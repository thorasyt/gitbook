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
    [1] = {item='water_bottle', count = 10},
    [2] = {item ='coffee', count = 10}
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

## newCharacter

This configuration defines validation rules
for new character creation fields.
### Configuration
```lua
Config.newCharacter = {
    ['firstname'] = {
        min = 2,
        max = 20
    },
    ['lastname'] = {
        min = 2,
        max = 20
    },
    ['height'] = {
        min = 150,
        max = 220,
        default = 185
    },
    ['dob'] = {
        minAge = 18
    }
}

```

##### Field Options

- **firstname**
  - Minimum length: `2`
  - Maximum length: `20`

- **lastname**
  - Minimum length: `2`
  - Maximum length: `20`

- **height**
  - Minimum: `150`
  - Maximum: `220`
  - Default value: `185`

- **dob**
  - Minimum age required: `18`
###### Behavior

- Validates input before character creation
- Prevents invalid or underage characters
- Applies default values when input is missing

#### Notes

- Field validation runs client-side and server-side
- Age is calculated from the date of birth
- Modify limits to match your roleplay rules

## animation

This configuration defines the animations used
by characters in the multicharacter selection scene.
Each animation consists of an animation dictionary
and an animation name.

### Configuration
```lua
Config.animation = {
    ['stand'] = {
        "anim@heists@heist_corona@single_team",
        "single_team_loop_boss"
    },
    ['sit'] = {
        "timetable@reunited@ig_10",
        "base_amanda"
    },
    ['standsmoke'] = {
        "amb@world_human_smoking@female@idle_a",
        "idle_b"
    },
    ['groundsit'] = {
        "anim@amb@business@bgen@bgen_no_work@",
        "sit_phone_phoneputdown_idle_nowork"
    },
    ['cooking'] = {
        "amb@prop_human_bbq@male@idle_a",
        "idle_a"
    },
    ['smokesit'] = {
        "switch@michael@parkbench_smoke_ranger",
        "parkbench_smoke_ranger_loop"
    }
}
Config.animation = {
    ['stand'] = {
        "anim@heists@heist_corona@single_team",
        "single_team_loop_boss"
    },
    ['sit'] = {
        "timetable@reunited@ig_10",
        "base_amanda"
    },
    ['standsmoke'] = {
        "amb@world_human_smoking@female@idle_a",
        "idle_b"
    },
    ['groundsit'] = {
        "anim@amb@business@bgen@bgen_no_work@",
        "sit_phone_phoneputdown_idle_nowork"
    },
    ['cooking'] = {
        "amb@prop_human_bbq@male@idle_a",
        "idle_a"
    },
    ['smokesit'] = {
        "switch@michael@parkbench_smoke_ranger",
        "parkbench_smoke_ranger_loop"
    }
}
```
#### Notes

- Animation keys are referenced using `animType`
- You can add custom animations if needed
- Make sure animation dictionaries exist in GTA

## Locations

This configuration defines spawn locations
used by the multicharacter selector.
Each location includes spawn position, camera paths,
and character preview positions.
### Configuration
```lua
Config.Locations = {
    ["golf"] = {
        coords = vec4(-1391.4323, 53.0910, 53.5987, 155.7444),

        camCoords = {
            vec4(-1405.7288, 73.7373, 52.9786, 267.2395),
            vec4(-1396.9381, 118.5998, 54.3559, 234.0813),
            vec4(-1375.0361, 158.9146, 57.2568, 198.3325),
            vec4(-1335.5956, 139.2919, 57.2705, 118.7829),
            vec4(-1328.3307, 100.9738, 56.1967, 148.4735),
            vec4(-1324.9789, 67.0208, 53.5307, 229.9875),
            vec4(-1324.6182, 59.5551, 53.5425, 91.7738),
            vec4(-1335.0964, 19.7804, 53.4545, 29.9474),
            vec4(-1362.2411, 4.6747, 53.1865, 333.9185),
            vec4(-1377.4480, 9.3734, 53.6545, 193.0871),
            vec4(-1374.8850, 24.9021, 53.6844, 14.7074),
            vec4(-1384.2885, 55.3650, 53.8402, 271.6217)
        },

        Characters = {
            [1] = {
                pedCoords = vec4(-1360.3391, 57.3336, 65.5148, 102.3301),
                camCoords = vec4(-1365.8866, 56.2222, 65.1424, 278.6035),
                animType = 'stand'
            },
            [2] = {
                pedCoords = vec4(-1314.4242, 122.8508, 56.8950, 329.6780),
                camCoords = vec4(-1312.9558, 125.1525, 57.3672, 144.0869),
                animType = 'sit'
            },
            [3] = {
                pedCoords = vec4(-1191.1241, 115.2740, 60.0645, 145.6757),
                camCoords = vec4(-1193.4943, 111.6696, 59.3142, 324.4308),
                animType = 'standsmoke'
            },
            [4] = {
                pedCoords = vec4(-1208.3478, 75.6107, 55.2944, 90.5322),
                camCoords = vec4(-1212.9581, 75.2195, 54.9530, 271.1544),
                animType = 'groundsit'
            }
        }
    }
}
```
###### Location Structure Explained

- `coords`  
  Base spawn location for the scene

- `camCoords`  
  Camera movement points for cinematic transitions

- `Characters`  
  Character preview positions with animations

#### Notes

- You can add unlimited locations
- Each character slot can have a different animation
- Animations must exist in `Config.animation`
- Restart the resource after modifying locations

## Nationalities

This configuration defines the list of nationalities
available when creating a new character.

### Configuration
```lua
Config.Nationalities = {
     "Afghanistan",
    "Åland Islands",
    "Albania",
    "American Samoa",
    "Algeria",
    "Andorra",
    "Antarctica",
    "Angola",
    "Antigua and Barbuda",
    "Anguilla",
    "Armenia",
    "Argentina",
    "Aruba",
    "Azerbaijan",
    "Australia",
    "Austria",
    "Bahamas",
    "Bangladesh",
    "Barbados",
    "Bahrain",
    "Belarus",
    "Belgium",
    "Benin",
    "Belize",
    "Bermuda",
    "Bonaire, Sint Eustatius and Saba",
    "Bosnia and Herzegovina",
    "Bolivia (Plurinational State of)",
    "Bouvet Island",
    "Bhutan",
    "Botswana",
    "Brazil",
    "British Indian Ocean Territory",
    "United States Minor Outlying Islands",
    "Virgin Islands (British)",
    "Brunei Darussalam",
    "Virgin Islands (U.S.)",
    "Bulgaria",
    "Burkina Faso",
    "Cameroon",
    "Cambodia",
    "Canada",
    "Cabo Verde",
    "Cayman Islands",
    "Central African Republic",
    "Burundi",
    "China",
    "Chile",
    "Christmas Island",
    "Cocos (Keeling) Islands",
    "Comoros",
    "Colombia",
    "Congo (Democratic Republic of the)",
    "Congo",
    "Costa Rica",
    "Cook Islands",
    "Cuba",
    "Croatia",
    "Curaçao",
    "Chad",
    "Czech Republic",
    "Denmark",
    "Djibouti",
    "Dominican Republic",
    "Dominica",
    "Cyprus",
    "Ecuador",
    "El Salvador",
    "Egypt",
    "Equatorial Guinea",
    "Estonia",
    "Ethiopia",
    "Eritrea",
    "Falkland Islands (Malvinas)",
    "Faroe Islands",
    "Fiji",
    "Finland",
    "France",
    "French Guiana",
    "French Polynesia",
    "French Southern Territories",
    "Gabon",
    "Gambia",
    "Georgia",
    "Germany",
    "Gibraltar",
    "Greece",
    "Greenland",
    "Grenada",
    "Ghana",
    "Guatemala",
    "Guam",
    "Guernsey",
    "Guinea-Bissau",
    "Guadeloupe",
    "Guinea",
    "Guyana",
    "Haiti",
    "Vatican City",
    "Heard Island and McDonald Islands",
    "Honduras",
    "Hungary",
    "Hong Kong",
    "Iceland",
    "Indonesia",
    "Ivory Coast",
    "Iran (Islamic Republic of)",
    "Iraq",
    "Ireland",
    "Isle of Man",
    "Italy",
    "India",
    "Israel",
    "Jersey",
    "Jamaica",
    "Jordan",
    "Kazakhstan",
    "Japan",
    "Kenya",
    "Kiribati",
    "Lao People's Democratic Republic",
    "Latvia",
    "Kyrgyzstan",
    "Lebanon",
    "Lesotho",
    "Kuwait",
    "Liberia",
    "Liechtenstein",
    "Libya",
    "Lithuania",
    "Luxembourg",
    "North Macedonia",
    "Malawi",
    "Madagascar",
    "Malaysia",
    "Macao",
    "Maldives",
    "Mali",
    "Malta",
    "Marshall Islands",
    "Mauritius",
    "Martinique",
    "Mauritania",
    "Mayotte",
    "Micronesia (Federated States of)",
    "Monaco",
    "Moldova (Republic of)",
    "Mexico",
    "Mongolia",
    "Montenegro",
    "Mozambique",
    "Montserrat",
    "Morocco",
    "Myanmar",
    "Namibia",
    "Nauru",
    "Netherlands",
    "New Zealand",
    "New Caledonia",
    "Nepal",
    "Nicaragua",
    "Nigeria",
    "Niger",
    "Korea (Democratic People's Republic of)",
    "Norfolk Island",
    "Northern Mariana Islands",
    "Niue",
    "Norway",
    "Pakistan",
    "Palau",
    "Palestine, State of",
    "Papua New Guinea",
    "Oman",
    "Panama",
    "Paraguay",
    "Philippines",
    "Peru",
    "Poland",
    "Portugal",
    "Puerto Rico",
    "Pitcairn",
    "Réunion",
    "Republic of Kosovo",
    "Qatar",
    "Romania",
    "Rwanda",
    "Russian Federation",
    "Saint Helena, Ascension and Tristan da Cunha",
    "Saint Barthélemy",
    "Saint Lucia",
    "Saint Pierre and Miquelon",
    "Saint Martin (French part)",
    "Saint Vincent and the Grenadines",
    "Samoa",
    "Saint Kitts and Nevis",
    "Saudi Arabia",
    "San Marino",
    "Sao Tome and Principe",
    "Senegal",
    "Serbia",
    "Seychelles",
    "Sierra Leone",
    "Sint Maarten (Dutch part)",
    "Slovakia",
    "Slovenia",
    "Singapore",
    "South Africa",
    "South Georgia and the South Sandwich Islands",
    "Solomon Islands",
    "Korea (Republic of)",
    "Sri Lanka",
    "Spain",
    "Somalia",
    "South Sudan",
    "Svalbard and Jan Mayen",
    "Suriname",
    "Swaziland",
    "Switzerland",
    "Syrian Arab Republic",
    "Taiwan",
    "Sudan",
    "Sweden",
    "Thailand",
    "Timor-Leste",
    "Tanzania, United Republic of",
    "Togo",
    "Tokelau",
    "Tajikistan",
    "Trinidad and Tobago",
    "Turkey",
    "Tunisia",
    "Turkmenistan",
    "Tonga",
    "Turks and Caicos Islands",
    "Ukraine",
    "Tuvalu",
    "United Arab Emirates",
    "United Kingdom of Great Britain and Northern Ireland",
    "Uganda",
    "United States of America",
    "Uruguay",
    "Uzbekistan",
    "Vanuatu",
    "Venezuela (Bolivarian Republic of)",
    "Wallis and Futuna",
    "Vietnam",
    "Western Sahara",
    "Zambia",
    "Zimbabwe",
    "Yemen"
}
```
##### Behavior

- Displays a selectable nationality list during character creation
- Stores the selected nationality with character data
- Can be freely customized or extended

#### Notes

- Order of entries affects display order
- Supports unlimited nationalities
- Restart the resource after modifying the list

## DefaultSkin

This configuration defines the default appearance
applied to new characters when they are created.
Separate presets are supported for male and female characters.

### Configuration
```lua
Config.DefaultSkin = {
    ["m"] = {
        -- Male default skin preset
        mom = 43,
        dad = 29,
        grandparents = 0,
        face_md_weight = 61,
        skin_md_weight = 27,
        face_g_weight = 0,
        nose_1 = -5,
        nose_2 = 6,
        nose_3 = 5,
        nose_4 = 8,
        nose_5 = 10,
        nose_6 = 0,
        cheeks_1 = 2,
        cheeks_2 = -10,
        cheeks_3 = 6,
        lip_thickness = -2,
        jaw_1 = 0,
        jaw_2 = 0,
        chin_1 = 0,
        chin_2 = 0,
        chin_13 = 0,
        chin_4 = 0,
        neck_thickness = 0,
        hair_1 = 76,
        hair_2 = 0,
        hair_color_1 = 61,
        hair_color_2 = 29,
        tshirt_1 = 4,
        tshirt_2 = 2,
        torso_1 = 23,
        torso_2 = 2,
        arms = 1,
        arms_2 = 0,
        pants_1 = 28,
        pants_2 = 3,
        shoes_1 = 70,
        shoes_2 = 2,
        beard_1 = 11,
        beard_2 = 10,
        beard_3 = 0,
        beard_4 = 0
    },

    ["f"] = {
        -- Female default skin preset
        mom = 28,
        dad = 6,
        grandparents = 0,
        face_md_weight = 63,
        skin_md_weight = 60,
        face_g_weight = 0,
        nose_1 = -10,
        nose_2 = 4,
        nose_3 = 5,
        chin_1 = -10,
        chin_2 = 10,
        neck_thickness = -5,
        hair_1 = 43,
        hair_2 = 0,
        hair_color_1 = 29,
        hair_color_2 = 35,
        tshirt_1 = 111,
        tshirt_2 = 5,
        torso_1 = 25,
        torso_2 = 2,
        pants_1 = 12,
        pants_2 = 2,
        shoes_1 = 20,
        shoes_2 = 10,
        eye_color = 8,
        moles_1 = 12,
        moles_2 = 8
    }
}
```
##### Behavior

- Applied automatically to newly created characters
- Used as a base before clothing or skin customization
- Works with supported clothing systems

### Notes

- Presets must match your skin/clothing resource
- Values are fully customizable
- Restart the resource after making changes
