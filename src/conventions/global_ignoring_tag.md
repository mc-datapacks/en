# Global Ignoring Tag (`***`)

## About

This convention provides a way for datapack to communicate with each other by specifying entity tags other datapacks can then look for. This is used heavily to prevent other datapacks from killing/removing custom entities from the world unexpectedly.

There are currently 4 ignoring tags: `global.ignore`, `global.ignore.pos`, `global.ignore.gui`, `global.ignore.kill`

### 1. `global.ignore.kill`

Any entity with this tag **must not** be killed by other datapack. This includes but not limited to `/kill` command.

```mcfunction
execute as @e[type=creeper, tag=!global.ignore, tag=!global.ignore.kill] run kill @s
```

### 2. `global.ignore.gui`

Any entity with this tag **must not** display visual effects around them. This includes but not limited to `/title` commands.

```mcfunction
execute as @a[tag=!global.ignore, tag=!global.ignore.gui] at @s run title @s actionbar [{"text": "Hello, World!", "color": "green"}]
```

> The exception of this is `/playsound`, `/tellraw` and `/particle` commands.

### 3. `global.ignore.pos`

Any entity with this tag **must not** be moved from where it was. This includes but not limited to `/tp`, `/teleport` commands.

```mcfunction
execute as @e[type=witch, tag=!global.ignore, tag=!global.ignore.pos] at @s run tp @s ~ ~0.1 ~
```

### 4. `global.ignore`

Any entity with this tag **must not** be included in the entity selector at all, you can also think of this tag as a combination of every ignoring tags above.

```mcfunction
execute as @e[tag=!global.ignore] at @s run function namespace:internal/logic/function
```

> This tag cannot be use on player selector (`@a`, `@e[type=player]`, `@p`, etc.)  
> However, entity selector that doesn't specifically run on the player (`@e[...]`) still need to filter out this tag.

## Note

The convention only applies if your function will change NBT of an unknown entity. if you are trying to change NBT of known entity (i.e. an entity with a special tag attached) you don't need to follow this convention.
