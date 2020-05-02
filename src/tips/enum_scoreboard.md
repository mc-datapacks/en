# Enum Scoreboard

You can use a fake player to represent a state of your scoreboard instead of using the number directly. This makes your code more verbose and increases maintainability for the future.

## Declaring enum

```
scoreboard players set #state.idle bb.enum 1
scoreboard players set #state.foo bb.enum 2
scoreboard players set #state.bar bb.enum 3
```
The enum can hold any value that you want as long as it's unique, we only care about the existence of the fake player itself.

## Using enum state

```
execute if score @s bb.state = #state.idle bb.enum run function <idle_state>
execute if score @s bb.state = #state.running bb.enum run function <running_state>
execute if score @s bb.state = #state.stopping bb.enum run function <stopping_state>
```
Here we check the state of `@s` and run appropriate function that must only be run in that state.
