# Shulker Box Inventory Manipulation

## About
This technique allows us to manipulate the player's inventory just like any other NBT using the shulker box's loot table.

## Concept
We take advantage of the fact that the shulker box can be configured to drop their content on the ground instead of dropping a shulker box containing its content, which can then be used by the `/loot` command to replace the player's inventory.

## Implementation

1\. To comply with the [Official Conventions](/conventions/index.md), you will have to modify the loot table of `minecraft:yellow_shulker_box` to the loot table below. The loot table will drop its content on the ground when it's mined by an item with NBT `{drop_contents: 1b}`. For compatibility reasons, `air` must be used as the item.  
[<center>https://pastebin.com/4sspBvep</center>](https://pastebin.com/4sspBvep)

2\. Create a placeholder shulker box that you will use to modify the player's inventory. Usually, this should be placed far away from the player's view, but for simplicity's sake, I'll place it at `~ ~ ~`.

```
setblock ~ ~ ~ minecraft:yellow_shulker_box
```

3\. You need to clone the player's inventory into some sort of NBT buffer, you can clone this into a data storage since they are faster.

```
data modify storage <storage> inventory set from entity <player> Inventory
```

4\. Due to a limited number of slots inside the shulker box, you have to process the NBT in "batches". The easiest way to organize these batches can be done by splitting it into "hotbar", "inventory", "armor" and "offhand" batches.
    <!-- - "hotbar" batch will contain items from slot 0 to 8 -->
    <!-- - "inventory" batch will contain items from slot 9 to 35 -->
    <!-- - "armor" batch will contain items from slot 100 to 103 -->
    <!-- - "offhand" btach will contain items from slot -106 (with the negative sign) -->

4.1. Create each batch using these commands.
```
                                      |--- Batch name                                     |---- Slot number
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 0b}]
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 1b}]
data modify storage <storage> batch.hotbar append from storage <storage> inventory[{Slot: 2b}]
.
.
.

// Don't forget to remove the `Slot` NBT from the item before moving to the next step!
data remove storage <storage> batch.hotbar[].Slot
```

4.2. Modify the NBT of each item however you like it to be.

5\. Copy the NBT from the previous step into the shulker box and then replace the player's inventory.

```
data modify block ~ ~ ~ Items set from storage <storage> batch.hotbar
loot replace entity <player> hotbar.0 9 mine ~ ~ ~ air{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.inventory
loot replace entity <player> inventory.0 27 mine ~ ~ ~ air{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.armor
loot replace entity <player> armor.feet 4 mine ~ ~ ~ air{drop_contents: 1b}

data modify block ~ ~ ~ Items set from storage <storage> batch.offhand
loot replace entity <player> weapon.offhand 1 mine ~ ~ ~ air{drop_contents: 1b}
```

Note: Notice the slots I used in each `/loot` command?

6\. Cleaning up

```
setblock ~ ~ ~ minecraft:air
data remove storage <storage> batch
data remove storage <storage> inventory
```

## Note

In this example, I assume that you need to modify NBT of all available slots, but if you want to, say, modify only items in your hotbar, then you don't need to use the unneeded batches, e.g. the "inventory", "armor" and "offhand" batches.
