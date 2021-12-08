# Global Forceload Tag (`***`)

## About

This convention requires data packs which force-load chunks to communicate their presence and ensure that chunks are only removed from being force-loaded when they do not need to be force-loaded by any data pack.

## Implementation

When running `/forceload add`, the programmer must spawn a tagged entity or tag an existing entity with `global.forceload` in the same chunk and within the same tick that the chunk was added. This entity's entire hitbox must reside in the chunk at all times.

-------------------

```mcfunction
forceload add ~ ~
summon marker ~ ~ ~ {Tags:["dp.example","global.forceload"]}
```

In the above example, a marker (which the datapack can find later with the `dp.example` tag) is summoned when we begin force-loading the chunk. 

And when running `/forceload remove`, the programmer must first un-tag or kill their tagged entity and check *at least* all blocks in a vanilla-sized chunk (y=-64 to 320) for any other entities tagged `global.forceload`. If an entity is found, the chunk should *remain* force-loaded.

-------------------

```mcfunction
# executed as our "marker" anywhere in the chunk
# "math" is a dummy objective

tag @s remove global.ignore

# Constant chunk width
scoreboard players set #16 math 16

# Align marker to X chunk border
execute store result score #pos math run data get entity @s Pos[0]
scoreboard players operation #pos math /= #16 math
execute store result entity @s Pos[0] double 16 run scoreboard players get #pos math

# Align marker to Z chunk border
execute store result score #pos math run data get entity @s Pos[2]
scoreboard players operation #pos math /= #16 math
execute store result entity @s Pos[2] double 16 run scoreboard players get #pos math

# Align marker to lowest Y block
data modify entity @s Pos[1] set value -64.0d

# Check region
execute at @s unless entity @e[tag=global.forceload,dx=15,dy=319,dz=15] run forceload remove ~ ~

kill @s
```

In the above example, we used the original entity (tagged `dp.example` in the earlier example), so we had to remove the `global.forceload` tag prior to checking the region. This isn't required and a new entity could be spawned in if the previous entity is still needed. 

We used some scoreboard math to move the entity to the negative-most edge of the chunk, then used volume selectors to test the region. As for why the volume selectors are one less than expected, see [MC-123441](https://bugs.mojang.com/browse/MC-123441).