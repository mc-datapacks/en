# Common Trait Convention (`**`)

## About

This convention is intended to bring the Ore Dictionary system from [Forge](http://files.minecraftforge.net/) to datapack's custom items. This is done by utilizing Item's custom NBT feature. We can insert any kind of NBT inside Item, which we can access using other commands.

With that, we can create a unified lookup system that every datapack can use to search for a specific item they want. You can then use the Common Trait Convention's provided syntax to construct a search function to find the item you need.

### Example Usage

It can be hard to visualize how this convention would be useful in the real world, so we compiled some useful usage that would not be possible without this convention.

1. Suppose you added a `custom furnace` which let you smelt copper ore into ingots. With this convention, you can detect for **any** copper ore from any datapacks to smelt it.
2. Suppose you added a `fridge` which only accepts food items. With this convention, you can detect **any** food items, even the custom ones from other datapacks.
3. Suppose you added a `custom anvil` which lets you repair tools straight from the material instead of the ingot. With this convention, you can detect **any** kind of material from other datapacks, even when the base material doesn't match.

### Traits

Traits represent behavior and properties that an object can have. By specifying these traits inside the item's NBT, other datapacks will be able to refer to that item via traits instead of item IDs directly.

Traits are a compound tag of strings to booleans and so will look like this in NBT (notice `traits: {...}`?)

```mcfunction
/give @s diamond{ctc: {traits: {"some": 1b, "trait": 1b, "here": 1b}, id: "example", from: "convention:wiki"}}
```

### Syntax

Common Trait Convention's syntax will be stored inside the `ctc` NBT of item. Inside `ctc` are the NBT tags: `id`, `from` and `traits`.

- `id`: The internal ID of your item. This doesn't matter outside your datapack, but *should* be unique within your datapack.
- `from`: A namespace specifying which datapack the item comes from.
- `traits`: A set of traits.

We will assume the following syntax is the NBT structure of `/give` command

```text
{
    ctc: {
        id: "my_copper_ore",
        from: "convention:wiki",
        traits: {"metal/copper": 1b, "block": 1b, "ore": 1b}
    }
}
```

> Minecraft does not have a `set` data type, so we replicate it using a compound tag.
> This means every trait must have a value of `1b` or `true` to stay consistent.

Let's look at these traits:

- `metal/copper`. This trait tells us that this item is copper.
- `block`. This trait tells us that this item is a placeable block.
- `ore`. This trait tells us that this item is an ore.

#### Slash Notation

In the above example, you will notice the use of `/` in `metal/copper`. This is used for categorization, when a name alone could be ambiguous or difficult to understand. For example, what would the trait `orange` mean? Is it the *color* orange or the *fruit* orange?

In such a case we'd use slash notation to separate them. `color/orange` and `fruit/orange`

## Usage

To detect or check for trait items you just need to check the `traits` NBT of the item.

> Detect if the player is holding a weapon

```mcfunction
execute as @a if entity @s SelectedItem.tag.ctc.traits."tool/weapon" run ...
```

This command detects if the item the player is holding has the trait `tool/weapon`.

---

> Detect if the container contains copper ore

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc.traits{"metal/copper": 1b, "ore": 1b} run ...
```

This command detects if the container contains an item with the traits `metal/copper` and `ore`

---

> Detect if the container contains a placeable item

```mcfunction
execute if block ~ ~ ~ Items[].tag.ctc.traits."block" run ...
```

This command detects if the container contains an item with the trait `block`.

---

> While quotes around the trait is not necessary in all cases, I always keep them there to stay consistent.

## Basic Traits

This is a provided list of traits that you can use. This doesn't mean you can't create new traits for your own use, but if there is a trait that suits your need in the list, you should use it.

> The list is split into multiple groups and you **should not** use traits from the same group twice.

### Object Type Group

This trait represents the state of the matter that this item holds.

|Trait|Description|
|-----|-----------|
|gas|Gaseous substance|
|liquid|Liquid substance|
|block|Placeable item|
|item|Normal minecraft item|

### Special Type Group

This group represents common traits from Minecraft.

> This group is an exception to the rule above, you can use multiple traits from this group as much as you like.

|Trait|Description|
|-----|-----------|
|ore|Ore block that can usually be found in caves|
|seed|Item that can be used to grow plants|
|flower|Flower item|
|grass|Block that can spread from one block to another|
|sapling|Block that can grow into a tree|
|vegetable|Food item that comes from `seed`|
|log|Item that drops from tree trunk|
|planks|Item that comes from processing `log`|

### Compression Group

This trait represents an item that can be combined to create a more compact version of itself, and vice versa.

For example:

- `redstone dust` -> `redstone block`
- `ice` -> `packed ice`
- `iron block` -> `iron ingot`

|Trait|Description|
|-----|-----------|
|packed|Most packed form of item, usually a block|
|ingot|Normal form of item, usually an ingot|
|nugget|Smallest form of item, usually a nugget|

### Edible Group

This trait represents an edible item that can be used by the player (drinking included).

|Trait|Description|
|-----|-----------|
|food|All types of edible item|

### Armor Group

This trait represents an item that can be worn by players and other entities.

|Trait|Description|
|-----|-----------|
|armor|All types of wearable item|

### Tool Sub-group

> This trait uses [Slash Notation](#slash-notation)!

This trait represents an item that can be used to interact with the world.

|Trait|Description|
|-----|-----------|
|tool/mining|This item can be used to mine stone-like blocks|
|tool/chopping|This item can be used to cut wooden blocks|
|tool/tilling|This item can be used to till soil|
|tool/watering|This item can be used to water soil|
|tool/weapon|This item can be used to fight monsters and other players|

### Gem Sub-group

> This trait uses [Slash Notation](#slash-notation)!

This trait represents any item that has a crystalline structure.

|Trait|Description|
|-----|-----------|
|gem/diamond|Diamond gemstone|
|gem/ruby|Ruby gemstone|
|gem/emerald|Emerald gemstone|
|gem/sapphire|Sapphire gemstone|
|gem/prismarine|Prismarine|
|gem/lapis|Lapis Lazuli gemstone|
|gem/obsidian|Any Obsidian material|
|gem/quartz|Any Quartz material|
|gem/opal|Opal gemstone|

### Metal Sub-group

> This trait use [Slash Notation](#slash-notation)!

This trait represents common metallic items.

|Trait|Description|
|-----|-----------|
|metal/iron|Item made up of iron|
|metal/gold|Item made up of gold|
|metal/copper|Item made up of copper|
|metal/aluminium|Item made up of aluminium|
|metal/tin|Item made up of tin|
|metal/silver|Item made up of silver|
|metal/lead|Item made up of lead|
|metal/nickel|Item made up of nickel|
|metal/platinum|Item that made up of platinum|

## Reference

- [Ore Dict](https://mcforge.readthedocs.io/en/1.12.x/utilities/oredictionary/)
