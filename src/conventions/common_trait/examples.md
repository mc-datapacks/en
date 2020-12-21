# CTC Examples

Traits are really just a normal NBT stored inside the item, nothing is special about them from the minecraft engine but we make this convention to standardize a way to communicate using these NBT.

There are two ways to use this nbt in your datapack, as a "producer" and a "consumer".
- For the producer, you will specify the necessary traits into your custom item and tell the consumer how to handle your custom item using those traits.
- For the consumer, you will write a code to check for those traits and handle them accordingly.

## Creating a custom item

Since trait is really just a normal NBT in an item. When you want to create a custom item you just need to attach the `ctc` field to it as well.

```
/give @s diamond_sword{CustomModelData: 1234567, ctc: {id: 'light_saber', from: 'example:light_saber', traits: {'tool/weapon': 1b, 'metal/platinum': 1b}}, display: {Name: 'Light Saber'} }
#                                 ^^^^^^^        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#                                  ^ You might want your custom item to have a custom texture         ^
#                                                                                                     ^ The ctc field
```

Or you can specify it in the loot table form like this

```json
{
	"pools": [
		{
			"rolls": 1,
			"entries": [
				{
					"type": "minecraft:item",
					"name": "minecraft:diamond_sword",
					"functions": [
						{
							"function": "minecraft:set_nbt",
							"tag": "{CustomModelData: 1234567}"
						},
						{
							"function": "minecraft:set_nbt",
							"tag": "{ctc: {id: 'light_saber', from: 'example:light_saber', traits: {'tool/weapon': 1b, 'metal/platinum': 1b}}}"
						}
					]
				}
			]
		}
	]
}
```

## Checking for trait item

This check is usually used for checking a general item that *might* be from other datapack as well.

Suppose we are creating a datapack that will display some special effect when the player is holding a weapon (i.e. health indicator, but that's left to the implementation).  
In this case, we can take advantage of the `tool/weapon` trait but do note that you also need to take vanilla's weapon into account as well.

We will write a function that is called on the player and check if it should display that special effect:
```mcfunction
# First, check for non-ctc vanilla items to see if the player is holding a weapon
execute unless data entity @s SelectedItem.tag.ctc if predicate example:is_holding_weapon run function example:display_special_effect

# Then check for the actual trait
execute if data entity @s SelectedItem.tag.ctc.traits{'tool/weapon': 1b} run function example:display_special_effect
```

> - `if predicate example:is_holding_weapon` is a pseudo-predicate that check whether the player is holding a weapon in their hand
> - `function example:display_special_effect` is a pseudo-function that execute the core feature of this datapack, the implementation of this function is irrelevant to this example.

## Checking for trait item inside your datapack

There are situations where you only want to check for your own custom item and not other similar trait items. In this case, you can just check for the `ctc.id` and `ctc.field` directly.

The rule for when to use a trait check or an explicit check is basically about whether it's "external" or "internal".  
Only use trait check for the function that wants to support other external items and only use explicit check for internal implementation that would cause a problem if it were other items.

## The Misconceptions

1. The convention **does not** have a limited set of traits you can use.  
The convention itself only enforce the syntax of the `ctc` field, the traits provided in the convention is simply a commonly used one but you are free to create new trait as you see fit.

2. The convention is an opt-in system where you can specify what kind of behavior do you want from other datapacks.  
You are **not** required to correctly set the traits of your custom item base on its material or texture. The choice of choosing the trait for a custom item is on the author of the datapack.
