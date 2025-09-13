# MinionLib Function Reference

## Complete Function Documentation

### Debugging & Logging Functions

#### `d(...)`
**Purpose**: Print variables or function results to console
**Parameters**: Any number of variables or functions
**Returns**: None
**Example**:
```lua
d("Hello World")
d(player.hp)
d("Player HP: " .. player.hp)
d(function() return "Dynamic message" end)
```

#### `stacktrace()`
**Purpose**: Print current call stack to console
**Parameters**: None
**Returns**: None
**Use Case**: Debugging function call flow
```lua
function myFunction()
    stacktrace() -- Shows how we got here
end
```

#### `ml_debug(string str)`
**Purpose**: Print debug message when logging is enabled
**Parameters**: `str` - Debug message string
**Returns**: None
**Note**: Only prints when `gEnableLog == "1"`
```lua
ml_debug("This only shows when debug logging is on")
```

#### `ml_error(string str)`
**Purpose**: Print error message to console
**Parameters**: `str` - Error message string
**Returns**: None
```lua
ml_error("Something went wrong: " .. errorMessage)
```

#### `ml_log(string str)`
**Purpose**: Add message to status bar line
**Parameters**: `str` - Status message string
**Returns**: None
**Note**: Shows on each pulse/update cycle
```lua
ml_log("Bot is running - HP: " .. player.hp .. "%")
```

### System Control Functions

#### `Exit()`
**Purpose**: Close current game instance
**Parameters**: None
**Returns**: None
**Use Case**: Emergency shutdown
```lua
if emergencyCondition then
    Exit()
end
```

#### `Reload()`
**Purpose**: Reload all Lua modules
**Parameters**: None
**Returns**: Boolean - Success status
**Use Case**: Hot-reload code changes
```lua
if Reload() then
    d("Modules reloaded successfully")
end
```

#### `Unload()`
**Purpose**: Try to unload the bot
**Parameters**: None
**Returns**: Boolean - Success status
```lua
if Unload() then
    d("Bot unloaded")
end
```

#### `Now()`
**Purpose**: Get current tick count
**Parameters**: None
**Returns**: Integer - Current time in ticks
**Use Case**: Timing operations
```lua
local startTime = Now()
-- Do something
local elapsed = TimeSince(startTime)
```

#### `TimeSince(integer previousTime)`
**Purpose**: Calculate time difference from previous time
**Parameters**: `previousTime` - Previous tick count
**Returns**: Integer - Time difference in ticks
```lua
local lastCheck = Now()
-- Later...
if TimeSince(lastCheck) > 1000 then -- 1 second
    lastCheck = Now()
    -- Do something every second
end
```

### Event System Functions

#### `RegisterEventHandler(string event, function handler, string name)`
**Purpose**: Register a handler for an event
**Parameters**:
- `event` - Event name to listen for
- `handler` - Function to call when event occurs
- `name` - Unique identifier for this handler
**Returns**: None
**Example**:
```lua
RegisterEventHandler("player_died", function(args)
    d("Player died: " .. tostring(args))
end, "MyDeathHandler")
```

#### `QueueEvent(string eventname, string args, ...)`
**Purpose**: Queue and fire an event
**Parameters**:
- `eventname` - Name of event to fire
- `args` - Event arguments (must be strings)
**Returns**: None
**Note**: All arguments must be strings
```lua
QueueEvent("custom_event", "arg1", "arg2", "arg3")
```

### Math Functions

#### `math.angle(table heading1, table heading2)`
**Purpose**: Calculate shortest angle between two headings
**Parameters**:
- `heading1` - First heading (table with x,y,z)
- `heading2` - Second heading (table with x,y,z)
**Returns**: Number - Angle between 0-180 degrees
```lua
local angle = math.angle({x=1,y=0,z=0}, {x=0,y=1,z=0})
```

#### `math.approxequal(number num1, number num2)`
**Purpose**: Check if two numbers are approximately equal
**Parameters**:
- `num1` - First number
- `num2` - Second number
**Returns**: Boolean
```lua
if math.approxequal(1.0, 1.001) then
    d("Numbers are approximately equal")
end
```

#### `math.crossproduct(table pos1, table pos2)`
**Purpose**: Calculate cross product of two positions
**Parameters**:
- `pos1` - First position vector
- `pos2` - Second position vector
**Returns**: Table - Cross product vector
```lua
local cross = math.crossproduct({x=1,y=0,z=0}, {x=0,y=1,z=0})
```

#### `math.distance2d(number x, number y, number x1, number y1)`
**Purpose**: Calculate 2D distance between two points
**Parameters**:
- `x, y` - First point coordinates
- `x1, y1` - Second point coordinates
**Returns**: Number - Distance
```lua
local dist = math.distance2d(0, 0, 3, 4) -- Returns 5
```

#### `math.distance3d(table pos1, table pos2)`
**Purpose**: Calculate 3D distance between two positions
**Parameters**:
- `pos1` - First position (table with x,y,z)
- `pos2` - Second position (table with x,y,z)
**Returns**: Number - Distance
```lua
local dist = math.distance3d(player.pos, target.pos)
```

#### `math.distance3d(number x, number y, number z, number x1, number y1, number z1)`
**Purpose**: Calculate 3D distance using individual coordinates
**Parameters**: Six numbers representing two 3D points
**Returns**: Number - Distance
```lua
local dist = math.distance3d(0, 0, 0, 1, 1, 1)
```

#### `math.distancepointline(table p1, table p2, table p3)`
**Purpose**: Calculate distance from point to line
**Parameters**:
- `p1, p2` - Points defining the line
- `p3` - Point to measure distance from
**Returns**: Number - Distance to line
```lua
local dist = math.distancepointline(lineStart, lineEnd, point)
```

#### `math.magnitude(table pos)`
**Purpose**: Calculate magnitude of position vector
**Parameters**: `pos` - Position vector (table with x,y,z)
**Returns**: Number - Vector magnitude
```lua
local mag = math.magnitude({x=3, y=4, z=0}) -- Returns 5
```

#### `math.round(number num, integer decimals)`
**Purpose**: Round number to specified decimal places
**Parameters**:
- `num` - Number to round
- `decimals` - Number of decimal places
**Returns**: Number - Rounded number
```lua
local rounded = math.round(3.14159, 2) -- Returns 3.14
```

#### `math.randomize(integer int)`
**Purpose**: Randomize a percentage value
**Parameters**: `int` - Percentage (0-100)
**Returns**: Integer - Randomized value near the percentage
```lua
local random = math.randomize(50) -- Returns value near 50%
```

### Navigation Functions

#### `PathDistance(table posTable)`
**Purpose**: Calculate total distance of a path
**Parameters**: `posTable` - Path table from NavigationManager
**Returns**: Number - Total path distance
**Example**:
```lua
local path = NavigationManager:GetPath(x1, y1, z1, x2, y2, z2)
local distance = PathDistance(path)
```

### Table Utility Functions

#### `table.merge(table table1, table table2, boolean keepExisting)`
**Purpose**: Merge two tables
**Parameters**:
- `table1` - First table (will be modified)
- `table2` - Second table (source)
- `keepExisting` - Optional, keep existing entries in table1
**Returns**: Table - Merged table
```lua
local merged = table.merge({a=1, b=2}, {b=3, c=4})
-- Result: {a=1, b=3, c=4}
```

#### `table.pairsbykeys(table table1, function sort)`
**Purpose**: Iterate table sorted by keys
**Parameters**:
- `table1` - Table to iterate
- `sort` - Optional custom sort function
**Returns**: Iterator
```lua
for key, value in table.pairsbykeys(mytable) do
    d(key .. " = " .. tostring(value))
end
```

#### `table.pairsbyvalue(table table1, function sort)`
**Purpose**: Iterate table sorted by values
**Parameters**:
- `table1` - Table to iterate
- `sort` - Optional custom sort function
**Returns**: Iterator
```lua
for key, value in table.pairsbyvalue(mytable) do
    d(key .. " = " .. tostring(value))
end
```

#### `table.print(table arg)`
**Purpose**: Print table contents line by line
**Parameters**: `arg` - Table to print
**Returns**: None
```lua
table.print(mytable)
```

#### `table.randomvalue(table arg)`
**Purpose**: Get random value from table
**Parameters**: `arg` - Table to get value from
**Returns**: Any - Random value from table
```lua
local random = table.randomvalue({1, 2, 3, 4, 5})
```

#### `table.shallowcopy(table arg)`
**Purpose**: Create shallow copy of table
**Parameters**: `arg` - Table to copy
**Returns**: Table - Shallow copy
```lua
local copy = table.shallowcopy(originalTable)
```

#### `table.size(table arg)`
**Purpose**: Get number of elements in table
**Parameters**: `arg` - Table to count
**Returns**: Number - Table size (0 if not a table)
```lua
local count = table.size(mytable)
```

#### `table.valid(table arg)`
**Purpose**: Check if table is valid and has entries
**Parameters**: `arg` - Table to validate
**Returns**: Boolean - True if valid table with entries
```lua
if table.valid(mytable) then
    d("Table is valid and has entries")
end
```

### HTTP Request Function

#### `HttpRequest(table params)`
**Purpose**: Make asynchronous HTTP request
**Parameters**: `params` - Request parameters table
**Returns**: None
**Parameters Table**:
- `host` - Server hostname
- `path` - Request path
- `port` - Server port
- `method` - HTTP method ("GET", "POST", "PUT", "DELETE")
- `https` - Boolean, use HTTPS
- `onsuccess` - Function to call on success
- `onfailure` - Function to call on failure
- `getheaders` - Boolean, return headers
- `body` - Request body (optional)
- `headers` - Request headers table (optional)

**Example**:
```lua
HttpRequest({
    host = "api.example.com",
    path = "/data",
    port = 443,
    method = "GET",
    https = true,
    onsuccess = function(data, header, status)
        d("Success: " .. data)
    end,
    onfailure = function(error, header, status)
        d("Failed: " .. error)
    end,
    getheaders = true
})
```

---

*Complete MinionLib Function Reference*
*All functions documented with examples and use cases*
