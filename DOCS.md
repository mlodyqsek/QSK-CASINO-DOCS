# QSK Casino - FiveM Casino Script

## Overview

QSK Casino is a comprehensive casino tablet system for FiveM servers, featuring multiple gambling games with full framework support for **QBCore**, **ESX**, and **Standalone** servers. The script provides a secure, feature-rich gambling experience with extensive customization options.

### Supported Frameworks
- **QBCore**: Integrates with QBCore's player data, economy, and permission systems
- **ESX**: Compatible with ESX Legacy, using player identifiers and society accounts
- **Standalone**: Runs without any framework dependency, using internal balance management

## Features

### Games Included
- **Crash**: Multiplayer crash game with real-time betting
- **Dice**: Target-based dice rolling with customizable odds
- **Coin Flip**: Simple heads/tails betting
- **Plinko**: Pinball-style dropping game with multiple board sizes
- **Mines**: Minesweeper-style game with variable mine counts
- **Gem Rush**: Slot machine with gem symbols
- **Pirate Rush**: Pirate-themed slot machine
- **Fruit Party**: Fruit-themed slot machine
- **HiLo**: Higher/Lower card prediction game
- **Blackjack**: Classic blackjack with dealer AI
- **Keno**: Number selection lottery game

### Framework Support
- **QBCore**: Full integration with QBCore economy and permissions
- **ESX**: Complete ESX compatibility with society accounts
- **Standalone**: Runs independently with internal balance system

### Security Features
- Rate limiting to prevent spam betting
- Input validation and sanitization
- Anti-cheat measures for client-side games
- Admin permission controls for chip management
- SQL injection protection through prepared statements
- License validation with feature gating (no remote kill-switches)
- Compliant with FiveM release rules (no obfuscation or hidden telemetry)

### Management Tools
- Comprehensive database logging (optional)
- Game statistics tracking
- Player balance history
- Promotion/challenge system
- Admin commands for chip management
- Live chat with moderation

### Customization
- Configurable bet limits and multipliers
- Adjustable house edge
- Customizable UI themes and colors
- Promotion challenges with rewards
- Chat filtering and moderation

## Installation

### Requirements
- FiveM server with MySQL database (MariaDB/MySQL 5.7+ recommended)
- One of the supported frameworks (QBCore, ESX Legacy, or standalone)
- Basic knowledge of Lua scripting and database management

### Steps
1. **Download and Extract**: Download the QSK-CASINO resource and extract it to your server's `resources` folder
2. **Database Setup**: Import the `database.sql` file into your MySQL database using a tool like phpMyAdmin, HeidiSQL, or command line:
   ```sql
   mysql -u username -p database_name < database.sql
   ```
3. **Framework Configuration**: Edit `config.lua` to match your server's framework (see Configuration section below)
4. **Server Configuration**: Add the following to your `server.cfg`:
   ```
   ensure QSK-CASINO
   ```
5. **Load Order**: Ensure QSK-CASINO loads AFTER your framework resource (QBCore/ESX)
6. **Restart Server**: Restart your FiveM server to load the resource

### Post-Installation Checks
- Check server console for any errors during startup
- Test the `/casino` command in-game
- Verify database tables were created successfully

## Configuration

The script is highly customizable through `config.lua`. Below are detailed explanations of each configuration option:

### License Configuration
```lua
Config.LicenseKey = "your-license-key-here"  -- Your license key from the developer
Config.ScriptName = "CASINo"                -- Must match the script name in license system
Config.LicenseAPI = "http://your-api-endpoint"  -- License validation API endpoint
Config.LicenseActive = false                -- Automatically set based on validation
Config.EnableTelemetry = false              -- Enable/disable telemetry (disabled for compliance)
```
- **LicenseKey**: Your unique license key provided by the developer
- **ScriptName**: Internal script identifier (do not change)
- **LicenseAPI**: The API endpoint for license validation (plain URL, no obfuscation)
- **LicenseActive**: Automatically managed; controls feature access
- **EnableTelemetry**: Telemetry is disabled by default for privacy and compliance

### Framework Settings
```lua
Config.Framework = 'qbcore' -- Options: 'qbcore', 'esx', 'standalone'
Config.QBCoreExport = 'qb-core-main' -- Only needed for QBCore, match your qb-core resource name
```
- **Framework**: Choose your server's framework. 'standalone' runs without any dependencies
- **QBCoreExport**: The exact name of your qb-core resource folder

### Database Settings
```lua
Config.DatabaseTable = 'casino_players'  -- Main player data table
Config.SaveBalanceOnLogout = true
```
- **DatabaseTable**: Name for the main player table (change if conflicts exist)
- **SaveBalanceOnLogout**: Whether to save player balances when they disconnect

### Database Tracking (Optional Logging)
```lua
Config.DatabaseTracking = {
    enableBetLogging = true,        -- Log all bets placed
    enableResultLogging = true,     -- Log bet results (wins/losses)
    enableBalanceHistory = true,    -- Log balance changes over time
    enablePlayerStats = true,       -- Track player statistics
    enableGameStats = true,         -- Track game-specific statistics
    enableChatLogging = false,      -- Log casino chat messages (privacy consideration)
    retentionDays = 30              -- Days to keep logs (0 = forever)
}
```
Enable/disable various logging features. Disable for better performance if not needed.

### Betting Limits
```lua
Config.MinBet = { dice = 1.0, mines = 1.0, ... }  -- Minimum bet per game
Config.MaxBet = { dice = 10000.0, mines = 10000.0, ... }  -- Maximum bet per game
Config.HouseEdge = 2.0  -- House edge percentage
```
- Set minimum and maximum bet amounts for each game
- House edge affects overall win probability (higher = more house advantage)

### Commands & Permissions
```lua
Config.Commands = {
    casino = 'casino', -- Command to open casino
    givechips = 'givechips', -- Admin command to give chips
    takechips = 'takechips' -- Admin command to take chips
}
Config.AdminGroups = { 'god', 'admin', 'superadmin' }  -- Framework permission groups
```
- Customize command names if they conflict with other resources
- AdminGroups should match your framework's permission system

### UI & Experience Settings
```lua
Config.EnableSound = true
Config.DefaultBalance = 1000 -- Starting balance for new players (standalone only)
Config.TransitionDelay = 0 -- Delay in ms when switching menus
Config.EnableChat = true -- Enable live chat feature
Config.EnableAnimations = true -- Enable UI animations
Config.MaxChatMessages = 50 -- Maximum chat messages to display
Config.Theme = 'dark' -- 'dark' or 'light'
```
- Sound: Enable/disable all audio effects
- DefaultBalance: Only applies in standalone mode
- Chat: Real-time chat between players in casino

### Chat Moderation
```lua
Config.Chat = {
    LogToDatabase = true, -- Log chat messages to database
    BadWords = {"fuck", "shit", "damn", "bitch"}, -- Words to filter
    CensorMode = "block", -- "block" = hide message, "censor" = replace with ******
    BypassPrevention = true -- Prevent character substitution bypasses
}
```
Configure chat filtering and moderation features.

### Colors & Styling
```lua
Config.AccentColor = '#0090ff' -- Default accent color (hex)
Config.AccentColorHover = '#0077d4' -- Hover state color
```
Change the accent colors used throughout the UI.

### Game-Specific Settings
Each game has individual configuration:
```lua
Config.Games = {
    dice = { enabled = true, name = 'Dice', minMultiplier = 1.01, maxMultiplier = 100.0 },
    mines = { enabled = true, name = 'Mines', minMines = 1, maxMines = 24 },
    plinko = { enabled = true, name = 'Plinko', rows = {8, 12, 16} },
    -- ... other games
}
```
- **enabled**: Set to false to disable specific games
- **name**: Display name in the menu
- Game-specific parameters (multipliers, mine counts, etc.)

### Promotions System
```lua
Config.Promotions = {
    Enabled = true,
    ResetInterval = 7, -- days
    Challenges = {
        {
            id = 'mines_16_win_100',
            game = 'mines',
            title = 'Mine Master',
            description = 'Win 100 times on 16-mine difficulty',
            target = 100,
            conditions = { mineCount = 16, win = true },
            reward = 'bonus_chips',
            rewardAmount = 500
        },
        -- ... more challenges
    }
}
```
- Enable/disable the entire promotion system
- Challenges reset every X days
- Fully customizable challenges with conditions and rewards

### Notifications
```lua
Config.Notifications = {
    insufficientFunds = 'Insufficient funds!',
    invalidBet = 'Invalid bet amount!',
    betPlaced = 'Bet placed successfully!',
    win = 'You won $%s!',
    loss = 'You lost!',
    -- ... more messages
}
```
Customize all in-game notification messages.

### Debug Mode
```lua
Config.Debug = false  -- Set to true only for development
```
Enable debug logging (use only for troubleshooting).

## Usage

### Player Commands
- `/casino` - Open casino menu
- `/[gamename]` - Open specific game (e.g., `/dice`, `/crash`)

### Admin Commands
- `/givechips [playerid] [amount]` - Give chips to player
- `/takechips [playerid] [amount]` - Remove chips from player

### Developer API

#### Client-Side Exports
- `exports['QSK-CASINO']:OpenCasino(gameName)` - Open casino menu or specific game
- `exports['QSK-CASINO']:CloseCasino()` - Close casino interface
- `exports['QSK-CASINO']:IsOpen()` - Check if casino is currently open

#### Server-Side Events
- `TriggerClientEvent('qsk-casino:openCasino', playerId, gameName)` - Open casino for specific player
- `TriggerEvent('qsk-casino:giveChips', playerId, amount)` - Give chips to player (admin)
- `TriggerEvent('qsk-casino:takeChips', playerId, amount)` - Take chips from player (admin)

#### Example Integration
```lua
-- Open casino menu for player
TriggerClientEvent('qsk-casino:openCasino', playerId)

-- Open specific game
TriggerClientEvent('qsk-casino:openCasino', playerId, 'crash')

-- Using exports
exports['QSK-CASINO']:OpenCasino('dice')
```

### Permissions
Configure admin groups in `config.lua` under `AdminGroups` for your framework.

## Database Schema

The script creates several tables for tracking:
- Player data and balances
- Bet history and results
- Balance changes
- Game statistics
- Chat logs (optional)
- Promotion progress

Automatic cleanup procedures remove old logs based on retention settings.

## Support

For support, please refer to the original documentation or contact the developer. The license system uses plain API endpoints for validation and does not include obfuscation or remote kill-switches for compliance with FiveM rules.

## License

This script is provided as-is for use in FiveM servers. Redistribution or resale requires permission from the original author. 
