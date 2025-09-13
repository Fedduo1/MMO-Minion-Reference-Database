# MMO Minion Reference Database

## Table of Contents
1. [Lua API Overview](#lua-api-overview)
2. [MinionLib Core Functions](#minionlib-core-functions)
3. [GUI API Reference](#gui-api-reference)
4. [Behavior Tree System](#behavior-tree-system)
5. [FFXIVMinion Specific API](#ffxivminion-specific-api)
6. [Quick Reference Guide](#quick-reference-guide)
7. [Local Installation Analysis](#local-installation-analysis)

---

## Lua API Overview

### Core Structure
All MMO Minion bots are written in Lua and use the MinionLib library as the foundation.

**Key Components:**
- **MinionLib**: Core library with GUI, utilities, and game interaction
- **Behavior Tree**: System for creating custom addons and automation
- **ShortcutManager**: Hotkey and shortcut management
- **FFXIVMinion**: Specialized FFXIV bot with combat, gathering, crafting

### Module Definition
```lua
[Module]
Name=YourAddon
Dependencies=minionlib
Version=1
Files=myluacode.lua
Enabled=1
```

---

## MinionLib Core Functions

### General Utility Functions

#### Debugging & Logging
```lua
d(...)                    -- Print variables/functions to console
stacktrace()             -- Print current call stack
ml_debug(string str)     -- Print when gEnableLog == "1"
ml_error(string str)     -- Print error messages
ml_log(string str)       -- Add to status bar line
```

#### System Control
```lua
Exit()                   -- Close current game instance
Reload()                 -- Reload all lua modules
Unload()                 -- Try to unload the bot
Now()                    -- Get current tick count
TimeSince(integer time)  -- Calculate time difference
```

#### Event System
```lua
RegisterEventHandler(event, handler, name)  -- Register event handler
QueueEvent(eventname, args, ...)            -- Queue and fire events
```

### Math Functions
```lua
math.angle(heading1, heading2)           -- Calculate angle between headings
math.distance2d(x, y, x1, y1)           -- 2D distance calculation
math.distance3d(pos1, pos2)             -- 3D distance calculation
math.magnitude(pos)                     -- Vector magnitude
math.round(num, decimals)               -- Round number
math.randomize(int)                     -- Randomize percentage (0-100)
```

### Navigation
```lua
PathDistance(posTable)  -- Calculate path distance
```

### Table Utilities
```lua
table.merge(table1, table2, keepExisting)  -- Merge tables
table.pairsbykeys(table, sortFunction)     -- Iterate sorted by keys
table.pairsbyvalue(table, sortFunction)    -- Iterate sorted by values
table.print(table)                         -- Print table contents
table.randomvalue(table)                   -- Get random value
table.shallowcopy(table)                   -- Shallow copy table
table.size(table)                          -- Get table size
table.valid(table)                         -- Check if valid table
```

---

## GUI API Reference

### Menu System Structure
The Minion Menu has 3 layers:
- **Components**: Main headers (require: expanded, name, id, optional: texture)
- **Members**: Rows under headers (require: name, id, optional: texture, tooltip, onClick, sort)
- **SubMembers**: Right-side submenus (require: name, id, optional: texture, tooltip, onClick)

### Menu API Functions
```lua
ml_gui.ui_mgr:AddComponent(table component)
ml_gui.ui_mgr:AddMember(table member, string componentid)
ml_gui.ui_mgr:AddSubMember(table submember, string componentid, string memberid)
```

### Example Menu Implementation
```lua
local ffxiv_mainmenu = {
    header = { 
        id = "FFXIVMINION##MENU_HEADER", 
        expanded = false, 
        name = "FFXIVMinion", 
        texture = GetStartupPath().."\\GUI\\UI_Textures\\ffxiv_shiny.png"
    },
    members = {}
}

ml_gui.ui_mgr:AddComponent(ffxiv_mainmenu)
ml_gui.ui_mgr:AddMember({
    id = "FFXIVMINION##MENU_DEV1", 
    name = "Dev1", 
    onClick = function() Dev.GUI.open = not Dev.GUI.open end, 
    tooltip = "Open the Dev monitor."
}, "FFXIVMINION##MENU_HEADER")
```

---

## Behavior Tree System

### Overview
Behavior Trees are used to create custom addons and automation logic. They provide a structured way to define bot behavior patterns.

### Key Concepts
- **Nodes**: Individual behavior components
- **Conditions**: Decision points in the tree
- **Actions**: Specific tasks to execute
- **Composites**: Nodes that control child execution

---

## FFXIVMinion Specific API

### Core Variables
```lua
ffxivminion.modes = {}                    -- Available modes
ffxivminion.modesToLoad = {}              -- Modes to load
ffxivminion.busyTimer = 0                 -- Busy state timer
ffxivminion.maxlevel = 100                -- Maximum level
ffxivminion.gameRegion = GetGameRegion()  -- Game region (1=JP, 2=CN, 3=NA/EU)
```

### Login System
```lua
ffxivminion.loginvars = {
    reset = true,
    loginPaused = false,
    datacenterSelected = false,
    serverSelected = false,
    charSelected = false,
    useLastLogin = false,
    useAutoLogin = false,
}
```

### Combat Routines
Each job has its own combat routine file:
- `ffxiv_combat_warrior.lua` - Warrior combat
- `ffxiv_combat_paladin.lua` - Paladin combat
- `ffxiv_combat_darkknight.lua` - Dark Knight combat
- And many more...

### Task System
```lua
ffxiv_task_grind.lua      -- Grinding automation
ffxiv_task_gather.lua     -- Gathering automation
ffxiv_task_craft.lua      -- Crafting automation
ffxiv_task_fish.lua       -- Fishing automation
ffxiv_task_fate.lua       -- FATE automation
ffxiv_task_party.lua      -- Party management
```

---

## Quick Reference Guide

### Essential Functions
| Function | Purpose | Example |
|----------|---------|---------|
| `d()` | Debug output | `d("Player HP: " .. player.hp)`
| `ml_log()` | Status bar message | `ml_log("Bot is running")`
| `Now()` | Current time | `local startTime = Now()`
| `TimeSince()` | Time difference | `if TimeSince(startTime) > 5000 then`
| `math.distance3d()` | Calculate distance | `local dist = math.distance3d(pos1, pos2)`

### Common Patterns
```lua
-- Event handling
RegisterEventHandler("eventname", function(args)
    d("Event triggered: " .. tostring(args))
end, "MyHandler")

-- Time-based checks
local lastCheck = 0
if TimeSince(lastCheck) > 1000 then
    lastCheck = Now()
    -- Do something every second
end

-- Distance calculations
local playerPos = Player.pos
local targetPos = Target.pos
local distance = math.distance3d(playerPos, targetPos)
```

---

## Local Installation Analysis

### Your FFXIVMinion Installation
**Location**: `F:\mm\Bots\FFXIVMinion64\`

#### Core Files
- `FFXIVMinion_64.dat` - Main bot executable
- `MinionLauncher.dat` - Bot launcher
- `gui.ini` - UI configuration and window positions

#### Key Directories
- `LuaMods/ffxivminion/` - Core bot functionality
- `LuaMods/ACR/` - Advanced Combat Routines
- `LuaMods/Husbando's Toolbox/` - Premium features
- `LuaMods/MadaoCore/` - Additional automation tools
- `GatherProfiles/` - Gathering automation profiles
- `CraftProfiles/` - Crafting automation profiles

#### Available Combat Routines
All jobs supported with optimized rotations:
- **Tanks**: Warrior, Paladin, Dark Knight, Gunbreaker
- **Melee DPS**: Monk, Dragoon, Ninja, Samurai, Reaper, Viper
- **Ranged DPS**: Bard, Machinist, Dancer, Pictomancer
- **Casters**: Black Mage, Summoner, Red Mage, Blue Mage
- **Healers**: White Mage, Scholar, Astrologian, Sage

#### Premium Addons Available
- **Husbando's Toolbox**: Comprehensive automation suite
- **MadaoCore**: Advanced botting features
- **TensorCore**: Combat analysis and optimization
- **KitanoiFuncs**: Custom functions and utilities

### Configuration Files
- `settings.db` - Main settings database
- `SettingsUUID.db` - Character-specific settings
- `gui.ini` - UI layout and window positions

---

## Useful Resources

### Official Documentation
- [MMO Minion Lua API](https://wiki.mmominion.com/doku.php?id=lua_api)
- [MinionLib Documentation](https://wiki.mmominion.com/doku.php?id=minionlib)
- [FFXIVMinion GitHub](https://github.com/MINIONBOTS/FFXIVMinion)
- [Behavior Tree System](https://wiki.mmominion.com/doku.php?id=behaviortree)

### Local Files to Explore
- `F:\mm\Bots\FFXIVMinion64\LuaMods\ffxivminion\ffxiv.lua` - Main bot logic
- `F:\mm\Bots\FFXIVMinion64\LuaMods\ffxivminion\class_routines\` - Combat routines
- `F:\mm\Bots\FFXIVMinion64\LuaMods\ffxivminion\GatherProfiles\` - Gathering profiles

---

*Last Updated: $(date)*
*Source: MMO Minion Wiki, GitHub Repository, Local Installation Analysis*
