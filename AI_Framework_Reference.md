# FFXIVMinion AI Framework Reference

## Overview
The FFXIVMinion AI Framework is a goal-driven behavior system that operates by scheduling a series of tasks designed to accomplish specific goals. This system provides modular, reusable components that can be combined to create complex automation behaviors.

---

## Core Concepts

### Goal Driven Behavior
- **Tasks**: Modular mini-states that accomplish specific goals
- **Completion/Fail Checks**: Determine when tasks should be de-queued
- **Subtask System**: Each task can have one active subtask
- **Natural Recursion**: Control passes through subtask hierarchy automatically

### Key Benefits
1. **Implicit State Changes**: No need for explicit state management
2. **Reduced Redundancy**: Higher-level tasks handle common checks
3. **Modularity**: Create reusable task components
4. **Guaranteed Sequencing**: Tasks control their subtasks directly

---

## Lua Object-Oriented Programming

### Inheritance System
```lua
-- Create new class inheriting from existing class
myNewClass = inheritsFrom(myOldClass)

-- Create instance with data members
myNewClass:Create()
    local newinst = inheritsFrom(myNewClass)
    newinst.member1 = 0
    newinst.member2 = false
end
```

### Method Notation
- **`:` (colon)**: Passes implicit "self" pointer - use for instance methods
- **`.` (dot)**: Static access - use for class methods

```lua
-- Correct usage
my_class:my_function()  -- self is available
my_class.my_function()  -- self is NOT available

-- Always use self for instance data
function my_class:my_function()
    self.value = 4  -- Correct
    my_class.value = 4  -- Wrong - accesses static class
end
```

---

## Data Structures

### ml_task_hub
**Purpose**: Controller for the entire framework
**Responsibilities**:
- Find highest priority queue with active task
- Pass update pulse to active task
- Add new tasks to queues
- Promote queued tasks from pending to active

### Task Queues
Three priority levels:
1. **IMMEDIATE_GOAL** - Highest priority (emergency tasks)
2. **REACTIVE_GOAL** - Medium priority (reactive tasks)
3. **GOAL** - Standard priority (normal tasks)

### ml_task
**Base class for all tasks**

#### Key Properties
- `subtask` - Currently active subtask
- `overwatch_elements` - Emergency condition checks
- `process_elements` - Task-specific condition checks

#### Core Methods
- `Update()` - Main task execution loop
- `ProcessOverWatch()` - Emergency condition handling
- `Process()` - Normal task processing
- `isValid()` - Task validity check
- `hasCompleted()` - Task completion check

---

## Task Execution Flow

### Update() Function Flow
```lua
function ml_task:Update()
    local continueUpdate = true
    while (continueUpdate) do
        -- 1. Check if task is still valid
        if (not self:isValid()) then
            self:DeleteSubTasks()
            return TS_FAILED
        end
        
        -- 2. Check if task has completed
        if (self:hasCompleted()) then
            self:DeleteSubTasks()
            return TS_SUCCEEDED
        end
        
        -- 3. Process emergency conditions (OverWatch)
        if(self:ProcessOverWatch()) then
            break
        end
        
        -- 4. Handle subtask if exists
        if (self.subtask ~= nil) then
            taskRet = self.subtask:Update()
            if (taskRet ~= TS_PROGRESSING) then
                continueUpdate = self:OnSubTaskReturn(ret)
                self:DeleteSubTasks()
            else
                continueUpdate = false
            end
        else
            -- 5. Process normal task logic
            continueUpdate = self:Process()
        end
    end
    return TS_PROGRESSING
end
```

### ProcessOverWatch()
**Purpose**: Handle emergency conditions that require immediate response
**When Called**: Before every task/subtask execution
**Use Cases**:
- Target moved out of range
- New aggro from another entity
- Target lost
- Player health critical

```lua
function ml_task:ProcessOverWatch()
    if (TableSize(self.overwatch_elements) > 0) then
        ml_cne_hub.clear_queue()
        ml_cne_hub.eval_elements(self.overwatch_elements)
        ml_cne_hub.queue_to_execute()
        return ml_cne_hub.execute()
    end
end
```

### Process()
**Purpose**: Handle normal task-specific logic
**When Called**: Only when no subtask is active
**Use Cases**:
- Combat skill selection
- Movement decisions
- Gathering actions
- Crafting sequences

```lua
function ml_task:Process()
    if (TableSize(self.process_elements) > 0) then
        ml_cne_hub.clear_queue()
        ml_cne_hub.eval_elements(self.process_elements)
        ml_cne_hub.queue_to_execute()
        ml_cne_hub.execute()
        return false
    end
end
```

---

## Task Return Values

### Task Status Constants
- **`TS_PROGRESSING`** - Task is still running
- **`TS_SUCCEEDED`** - Task completed successfully
- **`TS_FAILED`** - Task failed and should be removed

### Return Value Usage
- **`TS_PROGRESSING`**: Continue execution in next pulse
- **`TS_SUCCEEDED`**: Task complete, return to parent task
- **`TS_FAILED`**: Task failed, handle error and return to parent

---

## Creating Custom Tasks

### Basic Task Structure
```lua
-- Create new task class
my_custom_task = inheritsFrom(ml_task)

-- Initialize task
my_custom_task:Create()
    local newinst = inheritsFrom(my_custom_task)
    newinst.name = "My Custom Task"
    newinst.overwatch_elements = {}
    newinst.process_elements = {}
    return newinst
end

-- Define completion condition
my_custom_task.TASK_COMPLETE_EVAL = function()
    -- Return true when task is complete
    return some_condition
end

my_custom_task.TASK_COMPLETE_EXECUTE = function()
    -- Actions to take when task completes
    d("Task completed!")
end

-- Define failure condition
my_custom_task.TASK_FAIL_EVAL = function()
    -- Return true when task should fail
    return some_failure_condition
end

my_custom_task.TASK_FAIL_EXECUTE = function()
    -- Actions to take when task fails
    d("Task failed!")
end
```

### Adding Condition Elements
```lua
-- Add to overwatch_elements for emergency checks
table.insert(self.overwatch_elements, {
    name = "CheckPlayerHealth",
    eval = function() return Player.hp < 20 end,
    execute = function() 
        -- Emergency action
        ml_task_hub:AddTask(ml_task_rest.new())
    end
})

-- Add to process_elements for normal task logic
table.insert(self.process_elements, {
    name = "DoMainAction",
    eval = function() return true end,
    execute = function()
        -- Main task action
        d("Performing main action")
    end
})
```

---

## Common Task Patterns

### Combat Task Example
```lua
combat_task = inheritsFrom(ml_task)

combat_task:Create()
    local newinst = inheritsFrom(combat_task)
    newinst.name = "Combat Task"
    newinst.overwatch_elements = {}
    newinst.process_elements = {}
    
    -- Add emergency checks
    table.insert(newinst.overwatch_elements, {
        name = "CheckTargetValid",
        eval = function() return not Target.exists end,
        execute = function()
            -- Find new target or end combat
        end
    })
    
    -- Add combat logic
    table.insert(newinst.process_elements, {
        name = "ExecuteCombat",
        eval = function() return true end,
        execute = function()
            -- Combat skill selection and execution
        end
    })
    
    return newinst
end
```

### Movement Task Example
```lua
movement_task = inheritsFrom(ml_task)

movement_task:Create()
    local newinst = inheritsFrom(movement_task)
    newinst.name = "Movement Task"
    newinst.target_pos = nil
    newinst.overwatch_elements = {}
    newinst.process_elements = {}
    
    -- Add movement logic
    table.insert(newinst.process_elements, {
        name = "MoveToPosition",
        eval = function() 
            return math.distance3d(Player.pos, newinst.target_pos) > 2
        end,
        execute = function()
            -- Move towards target position
        end
    })
    
    return newinst
end
```

---

## Best Practices

### Task Design
1. **Single Responsibility**: Each task should have one clear purpose
2. **Fail Fast**: Check validity early in Update()
3. **Use OverWatch**: Put emergency checks in overwatch_elements
4. **Clear Completion**: Always define clear completion conditions

### Performance
1. **Minimize OverWatch**: Only essential emergency checks
2. **Efficient Process**: Keep process_elements focused
3. **Proper Cleanup**: Always call DeleteSubTasks() when done

### Safety
1. **Validity Checks**: Always verify objects exist before use
2. **Error Handling**: Implement proper failure conditions
3. **Resource Management**: Clean up resources when tasks end

---

## Integration with FFXIVMinion

### Task Hub Integration
```lua
-- Add task to queue
ml_task_hub:AddTask(my_custom_task.new())

-- Add high priority task
ml_task_hub:AddTask(my_emergency_task.new(), REACTIVE_GOAL)

-- Add immediate task
ml_task_hub:AddTask(my_immediate_task.new(), IMMEDIATE_GOAL)
```

### Common FFXIVMinion Tasks
- **`ml_task_grind`** - General grinding automation
- **`ml_task_gather`** - Gathering automation
- **`ml_task_craft`** - Crafting automation
- **`ml_task_combat`** - Combat automation
- **`ml_task_movement`** - Movement and navigation

---

## Advanced Concepts

### Task Chaining
```lua
-- Task A completes and queues Task B
function task_a:TASK_COMPLETE_EXECUTE()
    ml_task_hub:AddTask(task_b.new())
end
```

### Conditional Subtasks
```lua
-- Add subtask based on conditions
function my_task:Process()
    if (some_condition) then
        self.subtask = some_subtask.new()
    end
end
```

### Task Communication
```lua
-- Pass data between tasks
local data = {
    target_id = 12345,
    location = {x=100, y=200, z=300}
}
ml_task_hub:AddTask(my_task.new(data))
```

---

*FFXIVMinion AI Framework Reference*
*Complete guide to the task-based automation system*
