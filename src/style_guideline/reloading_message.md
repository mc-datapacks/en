# Reloading Message

You **must not** send an automatic message when the world is reloaded. It will clutter the chat unnecessarily and create a bad experience for the users.

Most of the time, the message will be an installation message, in which case you can use [Datapack Advancement](../conventions/datapack_advancement.md) instead.

> **Don't:** reload message for notification purposes that the normal players can see.

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
> **Case 2:** reload message for debugging purposes that the normal players shouldn't see.

```mcfunction
tellraw @a[name=Owner] <...>
tellraw @a[tag=convention.debug] <...>
```