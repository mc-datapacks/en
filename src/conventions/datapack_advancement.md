# Datapack Advancement Convention

## About

This convention aim to generallize installation message of datapacks into one place. This is done by utilizing [Advancement](https://minecraft.gamepedia.com/Advancements) tab to display installation message branching from the root advancement to datapack creator to the datapack itself.

The convention is split into 3 section: `Root`, `Namespace` and `Datapack`

## 1. Root Advancement

This is the advancement that every datapacks will branch off from. This advancement **must** be located at `/data/global/advancements/root.json` file.

```json
{
    "display": {
        "title": "Installed Datapacks",
        "description": "",
        "icon": {
            "item": "minecraft:knowledge_book"
        },
        "background": "minecraft:textures/block/gray_concrete.png",
        "show_toast": false,
        "announce_to_chat": false
    },
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

## 2. Namespace Advancement

This is the advancement allocated for *each* datapack creator, this advancement should be the same for all of *your* datapack. This advancement **should** be located at `/data/global/advancement/<namespace>.json`.

```json
{
    "display": {
        "title": "<Your name>",
        "description": "",
        "icon": {
            "item": "minecraft:player_head",
            "nbt": "{SkullOwner: '<your_minecraft_name>'}"
        },
        "show_toast": false,
        "announce_to_chat": false
    },
    "parent": "global:root",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

## 3. Datapack Advancement

This is an advancement for *your* datapack, this should be unique from your other Datapack Advancements. You can create this advancement anywhere as long as you don't pollute `/data/global/advancements/` folder.

```json
{
    "display": {
        "title": "<datapack name>",
        "description": "<datapack description>",
        "icon": {
            "item": "<item>"
        },
        "announce_to_chat": false,
        "show_toast": false
    },
    "parent": "global:<namespace>",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

### Note

everything encased inside `<...>` should be change to what is specify there.

## Result

Your advancement tab should now look something like this:

![Datapack Advancement Convention Preview](https://i.imgur.com/6bzBBr1.png)  
(Image by @Hashs#9531)
