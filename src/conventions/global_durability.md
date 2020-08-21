# Global Durability (`*`)

## About

This convention extends the functionality of the [Common Trait Convention](./common_trait.md) by providing a syntax for "custom durability" which can be useful for datapacks that add tools that have durability.

### Implementation

The convention uses the `ctc` tag from the [Common Trait Convention](./common_trait.md), as follows:

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
- `broken` is a boolean specifying whether the item is "broken" (i.e. [Broken Elytra](https://minecraft.gamepedia.com/Elytra)). This tag can be used in the case where you don't want to destroy the item when it runs out of durability.

## Note

The convention only enforces *where* the tag should be, it does not enforce the method by which you reduce durability. It also does not require that you have to keep `damage` and the vanilla `Damage` tag in sync.
