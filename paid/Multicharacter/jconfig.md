# Configuration
Configuration of tg-multicharacter script

## Framework
Framework of the server will be automatically fetch

## Defualt Slot
You can add slots according to your needs and set as many slots as you want. However, the total number of usable slots is not unlimited, as it depends on your Tebex package slots and the slot limits managed through Discord.
```lua Config.defualtSlot = 2```

## choosedLocation
This option allows you to select the active location from multiple predefined locations. All location data is configured in Config.Locations, where you can add, modify, or remove locations as needed.
```lua Config.choosedLocation = "golf"```

## SpawnSelector

You can enable or disable the Spawn Selector depending on your framework.
This system supports ESX, QBX, and QBCore.

Set the spawn selector location:
```lua Config.SpawnSelector = "golf"```
### ESX / QBX

For ESX and QBX, trigger the spawn selector in:
shared/client.lua
```lua function MultiCharacter:SpawnSelector(spawn, isNew, skin)
    -- Used for ESX and QBX
    -- Trigger your custom spawn selector here
end
```
### QBCore

For QBCore, trigger the spawn selector in:
shared/server.lua
```lua function Framework.SetQbSpawnTrigger(src, id, isNew)
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
