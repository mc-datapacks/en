# Global Ignoring Tag (`***`)

## About

This convention provides a way for datapacks to communicate with each other by specifying entity tags which other datapacks can then look for. This is used heavily to prevent other datapacks from killing/removing custom entities from the world unexpectedly.

There are currently 4 ignoring tags: `global.ignore`, `global.ignore.pos`, `global.ignore.gui` and `global.ignore.kill`.

### 1. `global.ignore.kill`

Any entity with this tag **must not** be killed by other datapacks. This includes, but is not limited to, the `/kill` command.

```mcfunction
execute as @e[type=creeper, tag=!global.ignore, tag=!global.ignore.kill] run kill @s
```

### 2. `global.ignore.gui`

Any entity with this tag **must not** display visual effects around them. This includes, but is not limited to, the `/title` command.

```mcfunction
execute as @a[tag=!global.ignore.gui] at @s run title @s actionbar [{"text": "Hello, World!", "color": "green"}]
```

> This excludes the `/playsound`, `/tellraw` and `/particle` commands.

### 3. `global.ignore.pos`

Any entity with this tag **must not** be moved positionally. This includes, but is not limited to, the `/tp` and`/teleport` commands.

```mcfunction
execute as @e[type=witch, tag=!global.ignore, tag=!global.ignore.pos] at @s run tp @s ~ ~0.1 ~
```

### 4. `global.ignore`

Any entity with this tag **must not** be included in an entity selector at all.

```mcfunction
execute as @e[tag=!global.ignore] at @s run function namespace:internal/logic/function
```

> This tag does not apply to player-only selectors (`@a`, `@e[type=player]`, `@p`, etc.)  

## Note

The convention only applies if your function will affect an unknown entity. If you are trying to target a known entity (e.g. an entity with a special tag attached specific to your datapack), you don't need to follow this convention.

--------------------

### **Note from Convention Mod (Optional)**

**FAQ**


> - **Q :** How can I use it? 
> - **A :** You can apply each tag to your entity for a specific purpose, 
> - **For example :**
>> - You don't want anyone to kill your entity. But there's no problem if it moves. `[ "faq.entity", "global.ignore", "global.ignore.kill" ]`
>> - This entity not use global ignoring tag since this is a temporary entity that is killed off right away.
>> ```mcfunction
>> summom area_effect_cloud ~ ~ ~ {Tags: ["faq.vector"], Age: 0, Duration: 1}
>> <...> kill @e[tag=faq.vector]
>> ```
>> - This is a "chunk marker" entity that shouldn't be move or kill by other datapack, so this entity must have `global.ignore`, `global.ignore.pos` and `global.ignore.kill`
>> ```mcfunction
>> summom area_effect_cloud ~ ~ ~ {Tags: `[ "faq.chunk_maker", "global.ignore", "global.ignore.pos", "global.ignore.kill" ]`, Age: 0, Duration: 2147483647}
>> ```
>> - Besides `as @e[tag=!global.ignore]`, `tp @e[tag=!global...]`, if you don't want the command to execute due entity `global.ignore` is in an area, you can use `if entity @e[tag=!global.ignore,distance=..30]` or `unless entity @e[tag=global.ignore,distance=..30]` and so on that you can be applied and not contrary to the convention.

> - **Q :** I don't want anyone to mess with my entity, I can tag it with `global.ignore` After using it I want to remove it but *"Any entity with this tag **must not** be included in an entity selector at all."* What do I need to do?
> - **A :** You can `["global.ignore", "Owner.tag"]` and then remove with `@e[tag=Owner.tag]`

> - **Q :** Can I use score to filter `!global.ignore`?
such as 
>```mcfunction
> scoreboard players set @e[tag=!global.ignore.kill] faq.pass 1
> kill @e[scores={faq.pass=1}]
>```
> - **A :** Yes, you can. But you need to make sure that your score isn't altered in any other way that you might accidentally do.

> - **Q :** If I'm not sure any `@s` has `!global.ignore` filtering at entry selector, can I use in `@s` instead?
> - **A :** Yes, you can. But don't worry, I can check it. And in some cases, you use global ignoring tags at entry selectors, it may cause the command to not work as you want. But then you find that using it in `@s` it works normally, you can consider use this method. Which you can note on why you use it before I'll review it. This is because in practice, use global ignoring tags at entry selectors results in less tag usage.
>```mcfunction
> # Examples of usage found
> # Pragmatic
> execute as @e[tag=!global.ignore,tag=!global.ignore.kill] run function <...>
>   ├ kill @s
>   ├ kill @s
>   ╰ Kill @s
>
> # Use in @s
> > execute as @e[tag=!global.ignore] run function <...>
>   ├ kill @s[tag=!global.ignore.kill]
>   ├ kill @s[tag=!global.ignore.kill]
>   ╰ Kill @s[tag=!global.ignore.kill]
> # From this situation it was found that before kill command, there are some commands that must execute with tagged entities global.ignore.kill. But there were no kills in any of the commands at that point.
>```

> - **Q :** Can I just only have `!global.ignore` in the `kill`, `tp` command? Because if that entity has `[ "faq.maker", "global.ignore", "global.ignore.pos", "global.ignore.kill" ]` it won't be selected anyway. 
> - **A :** Yes 1 ~~Yes 2~~ and *No* or **shouldn't**. 
>> - Yes 1 : If it has `!global.ignore` at entry selector This is the same as **# Use in @s**, but you also need to make sure that during execution (man in the middle), there are no changes to the executed `@s` entity like this -> `execute as @e[tag=!global.ignore] run kill @s[tag=!global.ignore.kill]`
>> - ~~Yes 2~~ you can and *No*: But you need to make sure that none of your entities you forgot to include `global.ignore`. And it can be an issue if some datapacks are certified, there is an entity that only `global.ignore.kill` or `global.ignore.pos` which may be caused by The reviewer overlooked or datapacker accidentally do.
>> - **Shouldn't :** **Please note that** if a problem occurs which is likely to arise from the case used in `@s` tag for whatever reason we have encountered such as **"Why put another tag too when there are only `!global.ignore` It just filters everything out, so it's useless"**, or **"due to many of tags"**, or **"because you're lazy"**, or whatever, That's not the datapack reviewer's fault. Then you come and tell us that **"We found these cases in your datapack, but we still certify them."** You can't say that either because sometimes we review what we get back. It is the reason that it is useless to the reason that we have given and disrespect for such advice. That means we'll assume you don't care in conventions at all. If it gives you trouble. Why don't you do them yourself? You say some conventions are useless and stupid to use. Why don't you debate those ideas and issues at #convention-ideas? Please reduce your ego and toxic. And reason why Datapack Reviewer often pass some cases? Not because of empathy but considering and analyzing some reasons, some cases are not so serious. But if there is any issue in the future, we cannot ignore it.