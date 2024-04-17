# DISCLAMER
> **This method is not recommanded at all. This is here because I know that a lot of people from the community want to use discord for logging but it will cause troubles with Discord.**

> Message from **Zoo**: **"Discord isnt a logging service, this will only get you in trouble with discord and cry whenever your server gets permanently rate limited" and I'm 100% with him on this point. I do not use Discord for my logs. This is simple to help the community but I do not encourage you to do it.**

> **I would recommand taking a $8/month subscription on https://www.fivemanage.com/pricing they offer a `Cheap Logs, Images, Videos & Voices Hosting Solution`. They have a complete Documentation that will help you to set it up easily.**

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
