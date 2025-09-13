# MMO Minion Lua API Quick Reference

## Essential Functions Cheat Sheet

### Debugging & Output
```lua
d("Hello World")           -- Print to console
d(variable)                -- Print variable value
d(function())              -- Print function result
ml_log("Status message")   -- Add to status bar
ml_error("Error message")  -- Print error
stacktrace()               -- Show call stack
```

### Time & Timing
```lua
local currentTime = Now()                    -- Get current tick
local timePassed = TimeSince(previousTime)   -- Calculate elapsed time
if TimeSince(lastAction) > 2000 then         -- Check if 2+ seconds passed
    -- Do something
end
```

### Math & Distance
```lua
local dist = math.distance3d(pos1, pos2)     -- 3D distance
local dist2d = math.distance2d(x, y, x1, y1) -- 2D distance
local angle = math.angle(heading1, heading2) -- Angle between headings
local mag = math.magnitude(position)         -- Vector magnitude
local rounded = math.round(number, 2)        -- Round to 2 decimals
```

### Events
```lua
-- Register event handler
RegisterEventHandler("eventname", function(args)
    d("Event received: " .. tostring(args))
end, "MyHandlerName")

-- Queue an event
QueueEvent("myevent", "arg1", "arg2")
```

### Tables
```lua
local merged = table.merge(table1, table2)           -- Merge tables
local size = table.size(mytable)                     -- Get table size
local valid = table.valid(mytable)                   -- Check if valid
local copy = table.shallowcopy(mytable)              -- Shallow copy
local random = table.randomvalue(mytable)            -- Random value
table.print(mytable)                                 -- Print table contents

-- Iterate sorted
for key, value in table.pairsbykeys(mytable) do
    d(key .. " = " .. tostring(value))
end
```

### Navigation
```lua
local path = NavigationManager:GetPath(x1, y1, z1, x2, y2, z2)
local distance = PathDistance(path)
```

### HTTP Requests
```lua
local params = {
    host = "api.example.com",
    path = "/endpoint",
    port = 443,
    method = "GET",
    https = true,
    onsuccess = function(data, header, status)
        d("Success: " .. data)
    end,
    onfailure = function(error, header, status)
        d("Failed: " .. error)
    end
}
HttpRequest(params)
```

## Common Patterns

### Basic Bot Loop
```lua
local lastUpdate = 0
local updateInterval = 1000 -- 1 second

function Update()
    if TimeSince(lastUpdate) > updateInterval then
        lastUpdate = Now()
        
        -- Your bot logic here
        d("Bot is running...")
    end
end
```

### Player/Target Information
```lua
local player = Player
local target = Target

if player and target then
    local distance = math.distance3d(player.pos, target.pos)
    d("Distance to target: " .. distance)
    
    if player.hp < 50 then
        ml_log("Player HP low: " .. player.hp .. "%")
    end
end
```

### Conditional Actions
```lua
-- Check conditions before acting
if Player.hp > 80 and Target.exists and math.distance3d(Player.pos, Target.pos) < 5 then
    -- Perform action
    d("Conditions met, performing action")
end
```

### Error Handling
```lua
local success, result = pcall(function()
    -- Risky operation
    return SomeFunction()
end)

if success then
    d("Success: " .. tostring(result))
else
    ml_error("Error: " .. tostring(result))
end
```

## FFXIVMinion Specific

### Mode Management
```lua
-- Check current mode
if ffxivminion.modes.current == "grind" then
    d("Currently grinding")
end

-- Switch modes
ffxivminion.modesToLoad = {"gather"}
```

### Combat Routines
```lua
-- Access job-specific combat data
local warrior = ffxiv_combat_warrior
if warrior.options then
    d("Rest HP: " .. warrior.options.settings.gRestHP)
end
```

### Task Management
```lua
-- Check if task is running
if ffxiv_task_grind.isrunning then
    d("Grind task is active")
end
```

## GUI Menu Creation

### Basic Menu Structure
```lua
local mymenu = {
    header = {
        id = "MYMENU##HEADER",
        expanded = false,
        name = "My Menu",
        texture = GetStartupPath().."\\GUI\\UI_Textures\\myicon.png"
    },
    members = {}
}

ml_gui.ui_mgr:AddComponent(mymenu)

-- Add menu items
ml_gui.ui_mgr:AddMember({
    id = "MYMENU##ITEM1",
    name = "Option 1",
    onClick = function()
        d("Option 1 clicked")
    end,
    tooltip = "This is option 1"
}, "MYMENU##HEADER")
```

## Best Practices

### Performance
- Use `TimeSince()` for timing instead of continuous checks
- Cache frequently accessed data
- Use `ml_debug()` instead of `d()` for frequent logging

### Safety
- Always check if objects exist before using them
- Use `pcall()` for risky operations
- Implement proper error handling

### Code Organization
- Use descriptive variable names
- Comment complex logic
- Break large functions into smaller ones
- Use consistent naming conventions

---

*Quick Reference for MMO Minion Lua API*
*Keep this handy while developing!*
