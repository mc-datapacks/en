# Shulker Box Inventory Manipulation

## About
This technique allows us to detect when a player is holding down right click.

## Concept
When a player is holding down right click while holding a carrot on a stick, Minecraft will take in another right click input after 4 ticks.<br/> 
Therefore a right click action is considered continuous if another right click is performed within 4 ticks.

## Result
You will be able to create something like this at the end. This datapack is the Example Datapack.<br/>
(Click image below to open a youtube video)
[![Hold-Right-Click Example](http://img.youtube.com/vi/ggjUPQ5BFkM/0.jpg)](http://www.youtube.com/watch?v=ggjUPQ5BFkM "HRC Example")

## Example Datapack
You can [download the example datapack here](http://www.mediafire.com/file/9tczxda3rz3c1bk/hold_rclick_example.zip/file)
Feels free to modify the example pack to be a starting point.<br/>   
Note: Example pack contains extra functionalities (Explosion spell) not explained here and are slightly different.<br/>
Note_2: Attribution appreciated but not required.<br/>

## Implementation
1\. The Setup<br/>
You will need 2 scoreboards.
(in: setup.mcfunction)
```
## This scoreboard objective increase by 1 for every right click using carrot on a stick 
scoreboard objectives add cc.ttr.coas minecraft.used:minecraft.carrot_on_a_stick
## This objective is a timer that gets reset to 5 on right click
scoreboard objectives add cc.ttr.last dummy
```
2\. Right Click Detection<br/>
Every time cc.ttr.coas is more than 1, we know that the player used a carrot on a stick, so we call the function tutorial:hold_detection<br/>
(in: main.mcfunction)
```
execute as @a[scores={cc.ttr.coas=1..}] run function tutorial:hold_detection
```
3\. Hold Right Click Initiation<br/>
Function tutorial:hold_detection sets cc.ttr.used to 0 so another right click can be detected again and sets cc.ttr.last to 5, meaning a carrot_on_a_stick has been used in the last 5 ticks.<br/>
(in: hold_detection.mcfunction)
```
scoreboard players set @s cc.ttr.used 0
scoreboard players set @s cc.ttr.last 5
```
We decrease cc.ttr.last by 1 every tick if it's more than 0.<br/>
(in: main.mcfunction)
```
scoreboard players remove @a[scores={cc.ttr.last=1..}] cc.ttr.last 1
```
4\. Hold Right Click Detection<br/>
If score cc.ttr.last is more than 0, that means a right click has been performed within 5 ticks, so we run the command we wanted.<br/>
Inversely, a right click is not considered held and we reset the score.<br/>
(in: main.mcfunction)
```
## More than 0, do stuffs.
execute as @a[scores={cc.ttr.last=1..}] run say Player Holding Right Click
## Less than 0, reset scores.
execute as @a[scores={cc.ttr.last=..0}] run say Player Not Holding Right Click
```
5\. That's all!<br/>
Happy Datapacking! Special thanks to Geegaz and TheSaltyPug!<br/>   
\- Cocoon