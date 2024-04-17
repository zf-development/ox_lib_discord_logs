# How to add Discord's Log to **ox_lib**

1. Go into `ox_lib/imports/logger/server.lua` and add this code:
```lua
if service == 'discord' then
    local webhook = GetConvar('inventory:webhook', '')

    function lib.logger(source, event, message, ...)
        PerformHttpRequest(webhook, function() end, 'POST', json.encode({
            username = cache.resource,
            embeds = {
                {
                    ['title'] = 'action: ' .. event or 'UNKNOWN EVENT' .. ' by source: ' .. source or 'UNKNOWN SOURCE' .. ' (' .. GetPlayerName(source) or 'UNKNOWN PLAYER NAME' .. ')',
                    ['footer'] = {
                        ['text'] = os.date('%c'),
                    },
                    ['description'] = message or 'UNKNOWN MESSAGE'
                }
            }
        }), { ['Content-Type'] = 'application/json' })
	end
end
```

2. Go in your `server.cfg` and add/replace this code:
```lua
set inventory:webhook "YOUR_DISCORD_WEBHOOK"
set ox:logger "discord"
```
