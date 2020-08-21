# Enum Scoreboard

You can use a fake player to represent a state on your scoreboard instead of using the number directly. This can make your code more readable and increase future maintainability.

## Declaring enum

```
scoreboard players set #state.idle bb.enum 1
scoreboard players set #state.foo bb.enum 2
scoreboard players set #state.bar bb.enum 3
```
The enum can hold any value that you want as long as it's unique. We only care about the fake players existing and having values that can be told apart.

## Using enum state

```
execute if score @s bb.state = #state.idle bb.enum run function <idle_state>
execute if score @s bb.state = #state.running bb.enum run function <running_state>
execute if score @s bb.state = #state.stopping bb.enum run function <stopping_state>
```
Here we check the state of `@s` and run a corresponding function based on its state. The benefit of using the enum pattern here is that we can assign names to the different expected states.
