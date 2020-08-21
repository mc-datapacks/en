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
