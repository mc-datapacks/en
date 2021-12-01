# Reloading Message

You **must not** send an automatic message when the world is reloaded. It will clutter the chat unnecessarily and create a bad experience for the users.

Most of the time, the message will be an installation message, in which case you can use [Datapack Advancement](../conventions/datapack_advancement.md) instead.

> **Example:** Use `load.json` call `setup` or `init` mcfunction that has tellraw command.
```json
{
    "values": [
        "convention:datapack/setup",
    ]
}
```
``convention:datapack/setup``
```mcfunction
tellraw @a <...>
```

--------------------

### **Note from Convention Mod (Optional)**

### **Exception cases**

> **Case 1:** a custom advancement to keep track of who has seen the message.
```json
{
  "criteria": {
    "trigger": {
      "trigger": "minecraft:tick"
    }
  },
  "rewards": {
    "function": "..."
  }
}
```
> **Case 2 :** Don't worry you can have tellraw commands in `setup` or `init`, but only for debugging purposes.

``convention:datapack/setup``
```mcfunction
tellraw @a[name=Owner] <...>
tellraw @a[tag=convention.debug] <...>
```