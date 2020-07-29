# Namespace Convention (`**`)

## About

This convention aims to prevent conflict when deciding the name for your scoreboards, functions, storages, etc. by using "namespace prefix".

## Implementation

Namespace prefix must be used whenever it is possible. This includes tags (datapack), tags (`/tag`), nbts, scoreboards, functions, advancements, loot tables, structures, world generation settings, recipes, data storage.

There is no rule specifying how you should design namespace prefix but these are some of the examples:

-------------------

```mcfunction
scoreboard objectives add bb.var dummy
```

In this example `bb` is the namespace which is shortened from `boomber` due to 16 characters limitation, I like to keep scoreboard's namespace low and then use `.` separator to quickly identify the namespace.

-------------------

```mcfunction
tag @s add boomber.foo.bar
```

In this example, I use the full `boomber` namespace because tag doesn't have a character limit.

--------------------

```mcfunction
data merge storage boomber:foo/bar {}
```

In this example data storage already have support for namespacing so I can take advantage of that right away.

--------------------

```mcfunction
give @s diamond{boomber: {custom_data: 123}}
```

In this example, I wrap the `custom_data` nbt inside my namespace to prevent any outside conflict entirely.

--------------------

The above example is my style of namespacing but there are many other styles that you can use as well, such as `namespace_foo`, `NAMESPACEfoo`, `namespace.foo`, `namespace:foo`, `namespace/foo`.

## Note

Using `minecraft` namespace to modify vanilla's behavior is a special case and it is allowed within a reasonable situation.  
However, setting `"replace"` to `true` in `tick.json` function tag is not a reasonable situation because that would prevent other datapacks from working.