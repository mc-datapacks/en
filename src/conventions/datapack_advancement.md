# Datapack Advancement Convention

## About

This convention aims to generalize the installation message of datapacks. This is done by using the [Advancement](https://minecraft.gamepedia.com/Advancements) tab to display installation message branching from the root advancement to datapack creator to the datapack itself.

The convention is split into 3 sections: `Root`, `Namespace` and `Datapack`

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

This is the advancement is for *each* datapack creator, it should be the same for all of *your* datapacks. It **should** be located at `/data/global/advancement/<namespace>.json`.

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

This is an advancement for *your* datapack, it should be unique from your other Datapack Advancements. You can create this advancement anywhere as long as you don't pollute `/data/global/advancements/` folder.

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

everything encased inside `<...>` should be changed to the specified information.

## Result

Your advancement tab should now look something like this:

![Datapack Advancement Convention Preview](https://i.imgur.com/6bzBBr1.png)  
(Image by @Hashs#9531)

## Standalone Datapack (Optional)

This is an optional syntax that you can use if you would like to display your datapack advancement in a standalone branch without any author name attached to it.

The advancement file or 'Standalone Datapack' **must** be created inside `/data/global/advancements/standalone/` directory and the advancement must branch off from 
the [Root Advancement](#1-root-advancement) directly with nothing in-between.
Branching more advancements after this one is possible, however.

> Keep in mind that if you are planning to release more datapacks you should use the normal syntax.
