# FFXIVMinion Local Installation Analysis

## Installation Overview
**Location**: `F:\mm\Bots\FFXIVMinion64\`
**Analysis Date**: Current
**Status**: Fully Installed with Premium Addons

---

## Core Executables

### Main Files
- **`FFXIVMinion_64.dat`** - Primary bot executable (64-bit)
- **`MinionLauncher.dat`** - Bot launcher application
- **`FFXIVMinionCN_64.dat`** - Chinese version
- **`FFXIVMinionKR_64.dat`** - Korean version

### Configuration Files
- **`gui.ini`** - UI layout, window positions, and interface settings
- **`settings.db`** - Main settings database (SQLite)
- **`SettingsUUID.db`** - Character-specific settings (SQLite)

---

## Directory Structure Analysis

### Core Bot Functionality
```
LuaMods/ffxivminion/
├── ffxiv.lua                    # Main bot logic and initialization
├── ffxiv_init.lua               # Bot initialization routines
├── ffxiv_helpers.lua            # Helper functions
├── ffxiv_helpers_gui.lua        # GUI helper functions
├── ffxiv_helpers_memoized.lua   # Memoized helper functions
├── ffxiv_common_cne.lua         # Common CNE functions
├── ffxiv_common_tasks.lua       # Common task functions
├── ffxiv_chatcodes.lua          # Chat code handling
├── ffxiv_music.lua              # Music system
├── ffxiv_skillmgr.lua           # Skill management
├── ffxiv_marker_mgr.lua         # Marker management
├── ffxiv_navigation.lua         # Navigation system
├── ffxiv_nav_data.lua           # Navigation data
├── ffxiv_radar.lua              # Radar system
├── ffxiv_shortcut_mgr.lua       # Shortcut management
├── ffxiv_unstuck.lua            # Unstuck system
└── class_routines/              # Job-specific combat routines
```

### Task System
```
LuaMods/ffxivminion/
├── ffxiv_task_assist.lua        # Party assistance
├── ffxiv_task_craft.lua         # Crafting automation
├── ffxiv_task_fate.lua          # FATE automation
├── ffxiv_task_fish.lua          # Fishing automation
├── ffxiv_task_gather.lua        # Gathering automation
├── ffxiv_task_grind.lua         # Grinding automation
├── ffxiv_task_huntlog.lua       # Hunt log automation
├── ffxiv_task_minigames.lua     # Mini-games automation
├── ffxiv_task_party.lua         # Party management
└── ffxiv_task_test.lua          # Testing framework
```

### Profile Directories
```
LuaMods/ffxivminion/
├── GatherProfiles/              # Gathering automation profiles
├── CraftProfiles/               # Crafting automation profiles
├── FishProfiles/                # Fishing automation profiles
├── QuestProfiles/               # Quest automation profiles
└── SkillManagerProfiles/        # Skill management profiles
```

---

## Combat Routines Available

### Tank Jobs
- **Warrior** (`ffxiv_combat_warrior.lua`) - Tank with high HP thresholds
- **Paladin** (`ffxiv_combat_paladin.lua`) - Balanced tank with healing
- **Dark Knight** (`ffxiv_combat_darkknight.lua`) - MP-based tank
- **Gunbreaker** (`ffxiv_combat_gunbreaker.lua`) - Combo-based tank

### Melee DPS
- **Monk** (`ffxiv_combat_monk.lua`) - Positional-based DPS
- **Dragoon** (`ffxiv_combat_dragoon.lua`) - Jump-based DPS
- **Ninja** (`ffxiv_combat_ninja.lua`) - Mudra-based DPS
- **Samurai** (`ffxiv_combat_samurai.lua`) - Kenki-based DPS
- **Reaper** (`ffxiv_combat_reaper.lua`) - Soul-based DPS
- **Viper** (`ffxiv_combat_viper.lua`) - Dual-wield DPS

### Ranged DPS
- **Bard** (`ffxiv_combat_bard.lua`) - Song-based DPS
- **Machinist** (`ffxiv_combat_machinist.lua`) - Turret-based DPS
- **Dancer** (`ffxiv_combat_dancer.lua`) - Dance-based DPS
- **Pictomancer** (`ffxiv_combat_pictomancer.lua`) - Art-based DPS

### Casters
- **Black Mage** (`ffxiv_combat_blackmage.lua`) - MP-based caster
- **Summoner** (`ffxiv_combat_summoner.lua`) - Pet-based caster
- **Red Mage** (`ffxiv_combat_redmage.lua`) - Dual-element caster
- **Blue Mage** (`ffxiv_combat_bluemage.lua`) - Spell-learning caster

### Healers
- **White Mage** (`ffxiv_combat_whitemage.lua`) - Pure healing
- **Scholar** (`ffxiv_combat_scholar.lua`) - Pet-based healing
- **Astrologian** (`ffxiv_combat_astrologian.lua`) - Card-based healing
- **Sage** (`ffxiv_combat_sage.lua`) - Barrier-based healing

### Crafting Jobs
- **Alchemist** (`ffxiv_crafting_alchemist.lua`)
- **Armorer** (`ffxiv_crafting_armorer.lua`)
- **Blacksmith** (`ffxiv_crafting_blacksmith.lua`)
- **Carpenter** (`ffxiv_crafting_carpenter.lua`)
- **Culinarian** (`ffxiv_crafting_culinarian.lua`)
- **Goldsmith** (`ffxiv_crafting_goldsmith.lua`)
- **Leatherworker** (`ffxiv_crafting_leatherworker.lua`)
- **Weaver** (`ffxiv_crafting_weaver.lua`)

### Gathering Jobs
- **Botanist** (`ffxiv_gather_botanist.lua`)
- **Miner** (`ffxiv_gather_miner.lua`)
- **Fisher** (`ffxiv_gather_fisher.lua`)

---

## Premium Addons Installed

### Husbando's Toolbox
**Location**: `LuaMods/Husbando's Toolbox/`
**Features**:
- Comprehensive automation suite
- Quest management
- Retainer management
- Market board automation
- Deep dungeon automation
- Eureka automation
- Gold Saucer automation
- Island Sanctuary automation

### MadaoCore
**Location**: `LuaMods/MadaoCore/`
**Features**:
- Advanced botting features
- Custom automation tools
- Enhanced UI components
- Additional safety features

### TensorCore
**Location**: `LuaMods/TensorCore/`
**Features**:
- Combat analysis and optimization
- Performance monitoring
- Advanced combat routines
- DPS analysis tools

### KitanoiFuncs
**Location**: `LuaMods/KitanoiFuncs/`
**Features**:
- Custom functions and utilities
- Dungeon automation
- Leve quest automation
- Vendor management
- Follow mode functionality

### ACR (Advanced Combat Routines)
**Location**: `LuaMods/ACR/`
**Features**:
- Enhanced combat routines
- Party management
- Advanced skill management
- Combat optimization

---

## Configuration Analysis

### GUI Configuration (`gui.ini`)
The GUI configuration shows extensive customization with:
- Multiple window positions and sizes
- Custom UI layouts
- Addon-specific windows
- Debug and development tools
- Style management

### Settings Databases
- **`settings.db`** - Global bot settings
- **`SettingsUUID.db`** - Character-specific configurations

### Hash Files
- **`GUI.csm`** - GUI component hashes
- **`LuaMods.csm`** - Lua module hashes
- **`MinionFiles.csm`** - Core file hashes
- **`Navigation.csm`** - Navigation data hashes

---

## Available Features

### Core Automation
- ✅ Combat automation for all jobs
- ✅ Gathering automation (Botanist, Miner, Fisher)
- ✅ Crafting automation (all 8 jobs)
- ✅ Quest automation
- ✅ FATE automation
- ✅ Hunt log automation
- ✅ Party management
- ✅ Navigation system
- ✅ Unstuck system
- ✅ Radar system

### Premium Features
- ✅ Deep dungeon automation
- ✅ Eureka automation
- ✅ Gold Saucer automation
- ✅ Island Sanctuary automation
- ✅ Retainer management
- ✅ Market board automation
- ✅ Advanced combat analysis
- ✅ Custom automation tools
- ✅ Enhanced UI components

### Safety Features
- ✅ Anti-detection measures
- ✅ Unstuck mechanisms
- ✅ Emergency stop functions
- ✅ Player detection
- ✅ GM detection
- ✅ Customizable safety settings

---

## Usage Recommendations

### Getting Started
1. **Launch**: Use `MinionLauncher.dat` to start the bot
2. **Configure**: Set up character-specific settings
3. **Test**: Start with simple tasks in safe areas
4. **Profile**: Create custom automation profiles

### Best Practices
1. **Start Simple**: Begin with basic combat or gathering
2. **Use Profiles**: Create job-specific automation profiles
3. **Monitor**: Keep an eye on bot behavior
4. **Safety First**: Enable all safety features
5. **Update**: Keep addons updated for best performance

### Advanced Usage
1. **Custom Scripts**: Modify Lua files for custom behavior
2. **Profile Creation**: Create custom automation profiles
3. **Integration**: Use multiple addons together
4. **Optimization**: Fine-tune settings for your playstyle

---

## File Locations for Customization

### Key Files to Modify
- `LuaMods/ffxivminion/ffxiv.lua` - Main bot logic
- `LuaMods/ffxivminion/class_routines/` - Combat routine customization
- `LuaMods/ffxivminion/GatherProfiles/` - Gathering profile creation
- `LuaMods/ffxivminion/CraftProfiles/` - Crafting profile creation

### Configuration Files
- `gui.ini` - UI customization
- `settings.db` - Global settings
- `SettingsUUID.db` - Character settings

---

*Local FFXIVMinion Installation Analysis*
*Complete feature inventory and usage guide*
