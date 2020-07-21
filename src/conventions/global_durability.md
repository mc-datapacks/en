# Global Durability (`*`)

## About

This convention extends the functionality of [Common Trait Convention](./common_trait.md) by providing a syntax for "custom durability" which can be useful for datapack that adds tools that can have durability.

### Implementation

The convention exists inside `ctc` tag provided by [Common Trait Convention](./common_trait.md) as follow

```json
{
    ctc: {
        tool: {
            damage: 0,
            durability: 0,
            broken: false
        }
    }
}
```

Where:

- `damage` is the current damage of the tool, functionally the same as vanilla's `Damage` tag counterpart.
- `durability` is the maximum durability of the tool or the maximum damage the tool can take.
- `broken` is a boolean specifying whether the item is "broken" (i.e. [Broken Elytra](https://minecraft.gamepedia.com/Elytra)), this tag can be used in the case where you don't want to destroy the item when it ran out of durability.

## Note

The convention only enforces *where* the tag should be, it does not enforce the method that you want to reduce their durability. And likewise, it also does not enforce that you have to keep `damage` and vanilla's `Damage` tag in-sync.
