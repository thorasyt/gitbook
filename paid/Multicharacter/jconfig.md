# Configuration
Configuration of tg-multicharacter script

## Framework
Framework of the server will be automatically fetch

## Defualt Slot
You can add slots according to your needs and set as many slots as you want. However, the total number of usable slots is not unlimited, as it depends on your Tebex package slots and the slot limits managed through Discord.
```lua 
Config.defualtSlot = 2
```

## choosedLocation
This option allows you to select the active location from multiple predefined locations. All location data is configured in Config.Locations, where you can add, modify, or remove locations as needed.
```lua 
Config.choosedLocation = "golf"
```

## SpawnSelector

You can enable or disable the Spawn Selector depending on your framework.
This system supports ESX, QBX, and QBCore.

Set the spawn selector location:
```lua 
Config.SpawnSelector = false
```
### ESX / QBX

For ESX and QBX, trigger the spawn selector in:
shared/client.lua
```lua 
function MultiCharacter:SpawnSelector(spawn, isNew, skin)
    -- Used for ESX and QBX
    -- Trigger your custom spawn selector here
end
```
### QBCore

For QBCore, trigger the spawn selector in:
shared/server.lua
```lua 
function Framework.SetQbSpawnTrigger(src, id, isNew)
    if Config.QbApartments then
        if isNew then
            if Config.SpawnSelector then
                -- Trigger custom spawn selector
            else
                TriggerClientEvent('apartments:client:setupSpawnUI', src, id)
            end
        else
            if Config.SpawnSelector then
                -- Trigger custom spawn selector
            else
                TriggerClientEvent('apartments:client:setupSpawnUI', src, id)
            end
        end
    else
        if isNew then
            TriggerClientEvent('tg-multicharacter:client:newchar', src)
        else
            if Config.SpawnSelector then
                -- Trigger custom spawn selector
            else
                TriggerClientEvent('qb-spawn:client:setupSpawns', src, id, false, nil)
                TriggerClientEvent('qb-spawn:client:openUI', src, true)
            end
        end
    end
end
```

#### Notes

- Config.SpawnSelector enables or disables the custom spawn selector
- ESX & QBX use client-side triggers
- QBCore uses server-side triggers
- Automatically falls back to default spawn UI when disabled

## logoutCommand

This command allows players to log out from their current character
and return to the multicharacter selection screen.

The command can be enabled or disabled from the configuration
and supports ESX, QBX, and QBCore frameworks.

### Configuration
```lua
Config.LogoutCommand = "logout"
```

#### Notes

- Make sure the multicharacter resource is running
- Some frameworks may require additional permission checks
- Can be disabled by setting Config.LogoutCommand to false or nil

## Clothing

This option defines which clothing system is used for character outfits.
The system supports multiple clothing resources and can be switched easily.
### Configuration
```lua 
Config.Clothing = 'illenium'

```

### Supported Clothing Systems

- illenium
- qb-clothing
- esx_skin (skinchanger)
- you can add more in shared/client at MultiCharacter:ApplySkinPed function

#### Notes

- Make sure the selected clothing resource is started before this script
- Changing this value does not require code edits
- Incorrect values may cause clothing menus not to open

## AutoDataBase

This option automatically manages required database changes for the script.
When enabled, the script will check and create missing columns or tables
without manual SQL execution.

### Configuration
```lua
Config.autoDataBase = true
```

##### Behavior

- Automatically checks required database structure on resource start
- Creates missing columns or tables if they do not exist
- Prevents startup errors caused by missing database fields

#### Notes

- Recommended to keep this enabled
- Requires MySQL/OxMySQL to be running
- Disable only if you prefer manual database management

## databaseTable

This option defines the database table used by the multicharacter system
to store character-related data.
### Configuration
```lua
Config.databaseTable = 'tg_multicharacter'
```
##### Behavior

- Uses the specified table to save multicharacter data
- Automatically creates or updates the table if `autoDataBase` is enabled
- Allows easy customization if you want to use a different table name
#### Notes

- Ensure the table name does not conflict with existing tables
- Changing this value requires the resource to restart
- Must match the database structure expected by the script

## TebexSlot

This option enables Tebex-based character slots.
When enabled, additional slots can be unlocked
using a valid Tebex purchase ID.
### Configuration
```lua
Config.TebexSlot = true
```
#### How It Works

- Players purchase a slot package from your Tebex store
- Tebex provides a unique purchase ID
- The purchase ID is validated by the script
- Once validated, the corresponding character slot is unlocked
##### Behavior

- Adds extra character slots linked to Tebex purchases
- Prevents slot usage without a valid purchase ID
- Works together with base slots and Discord-based slots
#### Notes

- Tebex integration must be properly configured
- Each purchase ID can only be used once
- Invalid or reused purchase IDs will be rejected

## DiscordSettings

This option enables Discord role-based configuration.
It is required for managing character slots and
character deletion permissions using Discord roles.
### Configuration
```lua
Config.DiscordSettings = true
```

#### What This Enables

- Role-based character slot limits
- Role-based permission to delete characters
- Secure access control using Discord roles

##### Behavior

- Checks player Discord roles on join
- Applies slot limits based on assigned roles
- Restricts character deletion based on role permissions
#### Notes

- Requires Discord identifiers to be enabled on the server
- Discord bot / role IDs must be correctly configured
- Additional Discord-related configuration options are defined below
## QbApartments

This option enables QBCore apartment spawning.
It is only applicable for QBCore and QBX frameworks
and works only when the custom Spawn Selector is disabled.
### Configuration
```lua
Config.QbApartments = true
```
##### Behavior

- Uses the default QBCore apartment spawn system
- Applies only to QBCore and QBX frameworks
- Automatically bypassed when `Config.SpawnSelector` is enabled

#### Conditions

- Framework must be QBCore or QBX
- `Config.SpawnSelector` must be set to `false`
#### Notes

- ESX does not support this option
- If disabled, the script falls back to the standard spawn system
- Restart the resource after changing this value

## StarterItems

This option defines the items that a new character
will receive when they are created for the first time.
### Configuration
```lua
Config.StarterItems = {
    'water_bottle',
    'coffee'
}
```
##### Behavior

- Items are granted only when a character is created
- Does not apply to existing characters
- Items are added directly to the player inventory
#### Notes

- Item names must match your inventory system
- Works with supported inventories (e.g. OX Inventory, QB Inventory)
- Restart the resource after modifying the item list

## defaultData

This option defines the default UI layout and color
settings used by the multicharacter interface.
### Configuration
```lua
Config.defaultData = {
    position = 'vertical',
    primaryColor = '#FF2900',
    secondaryColor = '#000000'
}
```
#### Options

- `position`  
  Controls the UI layout direction.  
  **Values:** `vertical`, `horizontal`

- `primaryColor`  
  Main accent color used throughout the UI.

- `secondaryColor`  
  Background or secondary UI color.
##### Behavior

- Applied automatically for new characters
- Can be overridden by saved player preferences
- Ensures consistent UI styling across the server
#### Notes

- Colors must be valid HEX values
- UI reload may be required after changes
- Restart the resource to apply default changes

## discordRoles

This configuration defines Discord roles that control
character slot access and character deletion permissions.
### Configuration
```lua
Config.discordRoles = {
    ['1223528758791766151'] = {
        slot = true,
        canDelete = true
    }
}
```
#### Options

- `slot`  
  Grants additional character slots to users with this Discord role.

- `canDelete`  
  Allows users with this Discord role to delete characters.

##### Behavior

- Player Discord roles are checked on join
- Permissions are applied automatically based on role ID
- Multiple roles can be configured with different permissions
#### Notes

- `Config.DiscordSettings` must be enabled
- Use Discord **Role IDs**, not role names
- Players must have their Discord linked and visible
## Musiclist

This option defines the background music list
used in the multicharacter UI.
You can add as many tracks as you want.
### Configuration
```lua
Config.Musiclist = {
    {
        name  = "Night Drive",
        file  = "song-1.mp3",
        cover = "song-1.jpg"
    }
}
```
###### Adding More Music

You can add multiple tracks by repeating the table entry:
```lua
Config.Musiclist = {
    {
        name  = "Night Drive",
        file  = "song-1.mp3",
        cover = "song-1.jpg"
    },
    {
        name  = "Sunset City",
        file  = "song-2.mp3",
        cover = "song-2.jpg"
    }
}
```

##### File Locations

- **Music files:**  
  `ui/music/mp3/`

- **Cover images:**  
  `ui/music/img/`

###### Supported Formats

- Audio: `.mp3`, `.ogg`
- Images: `.jpg`, `.png`
#### Notes

- File names must match exactly
- Restart the resource after adding new files
- Large audio files may increase loading time
