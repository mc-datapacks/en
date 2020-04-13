# Global Ignoring Tag

## About

This convention provide a way for datapack to communicate with each other by specifying entity tags which other datapacks can then look at. This is use heavily to prevent other datapacks from killing/removing custom entities from the world unexpectedly.

There are currently 4 ignoring tags: `global.ignore`, `global.ignore.pos`, `global.ignore.gui`, `global.ignore.kill`

### 1. `global.ignore.kill`

Any entity with this tag **must not** be kill by other datapack. This include but not limited to `/kill` command.

```mcfunction
execute as @e[type=creeper, tag=!global.ignore.kill] run kill @s
```

### 2. `global.ignore.gui`

Any entity with this tag **must not** display visual effect around them. This include but not limited to `/title`, `/particle`, `/playsound` commands.

```mcfunction
execute as @a[tag=!global.ignore.gui] at @s run title @s actionbar [{"text": "Hello, World!", "color": "green"}]
```

### 3. `global.ignore.pos`

Any entity with this tag **must not** be moved from where it was. This include but not limited to `/tp`, `/teleport` commands.

```mcfunction
execute as @e[type=witch, tag=!global.ignore.pos] at @s run tp @s ~ ~0.1 ~
```

### 4. `global.ignore`

Any entity with this tag **must not** be included in the entity selector at all, you can also think of this tag as a conbination of every ignoring tags above.

```mcfunction
execute as @e[tag=!global.ignore] at @s run function namespace:internal/logic/function
```

## Note

The convention only applies if your function will change NBT of unknown entity of course, if you are trying to change NBT of known entity (i.e. there's already a special tag attached to the entity) you don't need to do this.
