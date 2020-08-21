# Namespace Convention (`***`)

## About

This convention aims to prevent conflict when deciding the name for your scoreboards, functions, storages, etc. by using a "namespace prefix".

## Implementation

A namespace prefix must be used whenever it is possible. This includes tags (datapack), tags (`/tag`), nbts, scoreboards, functions, advancements, loot tables, structures, world generation settings, recipes, data storage.

There is no rule specifying how you should design your namespace prefix, but these are some of the examples:

-------------------

```mcfunction
scoreboard objectives add bb.var dummy
```

In this example `bb` is the namespace which is shortened from `boomber` due to 16 characters limitation. I like to keep the scoreboard namespace short and use `.` to seperate the namespace from the name.

-------------------

```mcfunction
tag @s add boomber.foo.bar
```

In this example, I use a full `boomber` prefix, because tags don't have a character limit.

--------------------

```mcfunction
data merge storage boomber:foo/bar {}
```

In this example data storage already has support for namespacing, so I don't need to use a prefix.

--------------------

```mcfunction
give @s diamond{boomber: {custom_data: 123}}
```

In this example, I wrap the `custom_data` nbt inside another tag, allowing it to act as a namespace.

--------------------

The above examples are my style of namespacing, but there are many other approaches that you can take, such as `namespace_foo`, `NAMESPACEfoo`, `namespace.foo`, `namespace:foo` and `namespace/foo`. Your imagination is the limit!

## Note

Using `minecraft` namespace to modify vanilla's behavior is a special case and it is allowed within a reasonable situation.  
However, for example, setting `"replace"` to `true` in the `tick.json` function tag is not a "reasonable situation", because that would prevent other datapacks from working.
