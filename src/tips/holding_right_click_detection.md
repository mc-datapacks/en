# Holding Right Click Detection by Cocoon

## About

This method allow you to detect when player is "holding" right click using carrot on a stick.

> Note: I will be referring to "carrot on a stick" as "coas" from now on

## Concept

To detect when player is holding right click you need to know when player right click `coas` *twice* next to each other.  
But because a single `coas` click need 2 ticks to process. To detect when player is holding you need atleast 4 ticks, which is a limitation of this method.

## Implementation

1\. Setup scoreboard objectives

```
#[setup]

#> This objective is used for detecting when player is right clicking.
scoreboard objectives add <coas> minecraft.used:minecraft.carrot_on_a_stick
scoreboard objectives add <timer> dummy
#> This objective is used to keep track of a "state machine"
scoreboard objectives add <state> dummy

#> Constants to define the possible states player can take.
scoreboard players set <state.idle> <state> 0 // Idling state
scoreboard players set <state.use> <state> 1 // Using coas state
```

> State Machine is a design pattern that separate multiple states into their own execution area without overlapping each other.

2\. Setup state machine code

> This assume that you've created a function loop that trigger this function on every players already!

```
#[main]

execute if score @s <state> = <state.idle> <state> run function [state/idle]
execute if score @s <state> = <state.use> <state> run function [state/use]
```

3\. Detect right click

```
#[state/idle]

execute if score @s <coas> matches 1.. run function [transition/use]
```

```
#[transition/use]

# Setup necessary scoreboard values
scoreboard players set @s <timer> 5
scoreboard players set @s <coas> 0
scoreboard players operation @s <state> = <state.use> <state>
```

4\. Handling holding right click

```
#[state/use]

scoreboard players remove @s <timer> 1

# Check when the timer ran out if player has right click coas again
execute if score @s <timer> matches 0 unless score @s <coas> matches 1.. run function [transition/idle]
execute if score @s <timer> matches 0 if score @s <coas> matches 1.. run function [transition/reset_timer]
```

```
#[transition/idle]

scoreboard players set @s <timer> 0
scoreboard players set @s <coas> 0
scoreboard players operation @s <state> = <state.idle> <state>
```

```
#[transition/reset_timer]

scoreboard players set @s <timer> 5
scoreboard players set @s <coas> 0
```

5\. As long as `[state/use]` is running player will be holding right click on coas. You can use that information to execute any functions that you want.

## Result

> A simple spell casting system where a spell will be casted when player hold right click for a period of time

![result](./holding_right_click_detection/result.gif)

## Note

- `<...>` is a placeholder value that you have to replace with your own value.
- `[...]` is a placeholder path to display relationship between each functions.

## Example Datapack

You can download the [example datapack](./holding_right_click_detection/example.zip) here.

This example pack contains extra code used to display the title message to player.
