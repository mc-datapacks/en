# Shulker Box Loot Table (`***`)

## About

This convention aims to make [Shulker Box Inventory Manipulation](../tips/shulker_box_inventory_manipulation.md) as conflict-free as possible by enforcing a specific loot table for the `minecraft:yellow_shulker_box` block.

## Implementation

Simply use this loot table inside `/data/minecraft/loot_tables/blocks/yellow_shulker_box.json` when you want to use the [Inventory Manipulation](../tips/shulker_box_inventory_manipulation.md) trick.

```json
{
  "type": "minecraft:block",
  "pools": [
    {
      "rolls": 1,
      "entries": [
        {
          "type": "minecraft:alternatives",
          "children": [
            {
              "type": "minecraft:dynamic",
              "name": "minecraft:contents",
              "conditions": [
                {
                  "condition": "minecraft:match_tool",
                  "predicate": {
                    "nbt": "{drop_contents: 1b}"
                  }
                }
              ]
            },
            {
              "type": "minecraft:item",
              "functions": [
                {
                  "function": "minecraft:copy_name",
                  "source": "block_entity"
                },
                {
                  "function": "minecraft:copy_nbt",
                  "source": "block_entity",
                  "ops": [
                    {
                      "source": "Lock",
                      "target": "BlockEntityTag.Lock",
                      "op": "replace"
                    },
                    {
                      "source": "LootTable",
                      "target": "BlockEntityTag.LootTable",
                      "op": "replace"
                    },
                    {
                      "source": "LootTableSeed",
                      "target": "BlockEntityTag.LootTableSeed",
                      "op": "replace"
                    }
                  ]
                },
                {
                  "function": "minecraft:set_contents",
                  "entries": [
                    {
                      "type": "minecraft:dynamic",
                      "name": "minecraft:contents"
                    }
                  ]
                }
              ],
              "name": "minecraft:yellow_shulker_box"
            }
          ]
        }
      ]
    }
  ]
}
```

## Reference

- [Shulker Box Inventory Manipulation](../tips/shulker_box_inventory_manipulation.md)
