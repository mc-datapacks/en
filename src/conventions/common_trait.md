# Common Trait Convention

## About

This convention intended to bring the Ore Dictionary system from [Forge](http://files.minecraftforge.net/) to datapack's custom item. This is done by utilizing Item's custom nbt feature. We can insert any kind of nbt inside item which we can use other commands to check/look into it.

With that, we can create a unified lookup system that every datapacks can use to search for a specific item they want. You can then use the Common Trait Convention's provided syntax to construct a search function to find the item you need.

### Example Usage

It can be hard to visualize how this convention would be useful in the real world, so we compile some useful usage that would not be possible without this convention.

1. Suppose you added a `custom furnace` which let you smelt copper ore into ingots. With this convention, you can detect for **any** copper ore from any datapacks to smelt it.
2. Suppose you added a `fridge` which only accept food items, with this convention you can detect for **any** food items even the custom ones from other datapack.
3. Suppose you added a `custom anvil` which lets you repair tools straight from the material instead of the ingot, with this convention you can detect **any** kind of material from other datapacks even when the base material doesn't match.

### Traits

Traits are behavior and properties that an object can have. By specifying these traits inside the item's nbt. Other datapacks will be able to refer to that item via traits instead of item id directly.

Trait is an array of strings and so will look like this in nbt (notice `traits: [...]`?)

```mcfunction
/give @s diamond{ctc: {traits: ["some", "trait", "here"], id: "example", from: "convention:wiki"}}
```

### Syntax

Common Trait Convention's syntax will be stored inside `ctc` nbt of item. inside`ctc` contains: `id`, `from` and `traits` nbts.

- `id`: internal id of your item, this should not be used outside of your datapack but should be unique within your datapack.
- `from`: a namespace specifying which datapack is the item comes from.
- `traits`: an array of traits that you can use to refer to items outside of your datapack.

We will assume the following syntax is the NBT structure of `/give` command

```text
{
    ctc: {
        id: "my_copper_ore",
        from: "convention:wiki",
        traits: ["metal/copper", "block", "ore"]
    }
}
```

Let's look at `traits` nbt

- `metal/copper`, this trait tells us that this item is copper.
- `block`, this trait tells us that this item is a placeable block.
- `ore`, this trait tells us that this item is an ore.

#### Slash Notation

In the above example, you will notice the use of `/` in `metal/copper`, this is used when the trait alone can be ambiguous. For example, what does trait `orange` mean? is it the *color* orange or the *fruit* orange?

In such case we'll use slash notation to separate them. `color/orange` and `fruit/orange`

## Usage

To detect or check for trait items you just need to check for `traits` nbt of the item.

> Detect if the player is holding a weapon

```mcfunction
execute as @a if entity @s SelectedItem.tag.ctc{traits: ["tool/weapon"]} run ...
```

> Detect if the container contains copper ore

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc{traits: ["metal/copper", "ore"]} run ...
```

> Detect if the container contains a placeable item

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc{traits: ["block"]} run ...
```

## Basic Traits

This is a provided list of traits that you can use, this doesn't mean you can't create new traits for your own use but if there is a trait that suits your need in this you should use it instead.

> The list is split into multiple groups and you **should not** use traits from the same group twice.

### Object Type Group

This trait represents the state of matter that this item holds.

|Trait|Description|
|-----|-----------|
|gas|Gaseous substance|
|liquid|Liquid substance|
|block|Placeable item|
|item|Normal minecraft item|

### Special Type Group

This trait represents common behavior from modded minecraft, this should help with integrating your pack into this convention.

> This group is an exception to the rule above, you can use multiple traits from this group as much as you like.

|Trait|Description|
|-----|-----------|
|ore|Ore block that can usually be found in cave|
|seed|Item that can be used to grow plant|
|flower|Flower item|
|grass|Block that can spread from one block to another|
|sapling|Block that can grow into tree|
|vegetable|Food item that comes from `seed`|
|log|Item that drops from tree|
|planks|Item that come from processing `log`|

### Compression Group

This trait represents an item that can be combined to create a more compact version of itself and vice versa

For example:

- `redstone dust` -> `redstone block`
- `ice` -> `packed ice`
- `iron block` -> `iron ingot`

|Trait|Description|
|-----|-----------|
|packed|Most packed form of item, usually be a block form of this item|
|ingot|Normal form of item, usually be an ingot form of this item|
|nugget|Smallest form of item, usually be a nugget form of this item|

### Edible Group

This trait represents an edible item that can be used by the player (drinking included)

|Trait|Description|
|-----|-----------|
|food|All types of edible item|

### Armor Group

This trait represents an item that can be worn by players and other entities

|Trait|Description|
|-----|-----------|
|armor|All types of wearable item|

### Tool Sub-group

> This trait use [Slash Notation](#slash-notation)!

This trait represents an item that can be used to interact with the world.

|Trait|Description|
|-----|-----------|
|tool/mining|This item can be used to mine a stone block and other related blocks|
|tool/chopping|This item can be used to cut trees and wood material|
|tool/tilling|This item can be used to till the soil|
|tool/watering|This item can be used to water the soil|
|tool/weapon|This item can be used to fight monsters and other players|

### Gem Sub-group

> This trait use [Slash Notation](#slash-notation)!

This trait represents any item that have crystalline structure.

|Trait|Description|
|-----|-----------|
|gem/diamond|Diamond gemstone|
|gem/ruby|Ruby gemstone|
|gem/emerald|Emerald gemstone|
|gem/saphire|Saphire gemstone|
|gem/prismarine|Prismarine|
|gem/lapis|Lapis Lazuli gemstone|
|gem/obsidian|Any Obsidian material|
|gem/quartz|Any Quartz material|
|gem/opal|Opal gemstone|

### Metal Sub-group

> This trait use [Slash Notation](#slash-notation)!

This trait represents the metallic items that are often added by mods

|Trait|Description|
|-----|-----------|
|metal/iron|Item that made up of iron|
|metal/gold|Item that made up of gold|
|metal/copper|Item that made up of copper|
|metal/aluminium|Item that made up of aluminium|
|metal/tin|Item that made up of tin|
|metal/silver|Item that made up of silver|
|metal/lead|Item that made up of lead|
|metal/nickle|Item that made up of nickle|
|metal/platinum|Item that made up of platinum|

## Reference

- [Ore Dict](https://paleocrafter-mcforge.readthedocs.io/en/fix-mkdoc-update/utilities/oredictionary/)
