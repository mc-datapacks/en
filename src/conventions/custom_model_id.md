# Custom Model ID (`***`)

## About

This convention aims to reduce conflict within the [custom model data](https://minecraft.gamepedia.com/Model#Item_tags) system as much as possible by assigning a unique id for every creator to use.

## 1. Register your id

"id" is an integer between 1-999 which we'll use to uniquely namespaced custom model data to prevent conflict between [resourcepacks](https://minecraft.gamepedia.com/Resource_pack).

You can register your id at [https://mc-datapacks.web.app](https://mc-datapacks.web.app/custom_model_id).

## 2. Prefix your model with your id

You will prefix your custom model data with your id in this format where `XXX` is your ID and `0000` is any custom model data that fits within 4 digits.

|id|cmd|
|---|----|
|XXX|0000|

Here are some examples:

### 2.1. id = 42

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 420001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 420020}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 420300}, "model": "path/to/model/3"}
    ]
}
```

### 2.2. id = 808

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 8081001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 8082002}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 8083003}, "model": "path/to/model/3"}
    ]
}
```

### 2.3. id = 1

```json
{
    "overrides": [
        {"predicate": {"custom_model_data": 10001}, "model": "path/to/model/1"},
        {"predicate": {"custom_model_data": 10010}, "model": "path/to/model/2"},
        {"predicate": {"custom_model_data": 10011}, "model": "path/to/model/3"}
    ]
}
```
