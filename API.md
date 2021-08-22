Mechanization is modularized into a set of event calls and scoreboard information. As such, it is very easy to develop add-ons that use Mechanization's features to create a seamless experience. This page documents everything you might find useful in creating an add-on.

### Important References:

Mechanization depends on Datapack Utils for things like crafting, smelting, custom tools and world generation. Since Mech depends on DU, you can use all of its API in your project as well. You can get more information over on its [wiki](https://github.com/ImCoolYeah105/Datapack-Utilities/wiki).

Mechanization follows (as closely as possible) the conventions specified by the Minecraft Datapacks community, which helps to maintain compatibility between datapacks. You can reference them here: https://mc-datapacks.github.io/en/

## Index

* [Energy API](https://github.com/ICY105/Mechanization/wiki/API#energy-api): how to interface with the power grid and portable batteries
* [Liquids API](https://github.com/ICY105/Mechanization/wiki/API#liquids-api): how to interact with pipes and liquid storing blocks
* [Pneumatic Tubing Interactions](https://github.com/ICY105/Mechanization/wiki/API#liquids-api): how to interact with pipes and liquid storing blocks
* [General Data](https://github.com/ICY105/Mechanization/wiki/API#general-data): lists of entity tags, scoreboard data, and function tags

# Energy API

Mechanization's foremost feature is the energy grid. This is conducted wirelessly by batteries and/or capacitors. To add a machine to the energy grid, summon an entity with one of the following tags. Then, set its score `mech_power` to 0

```
mech_transmitter: Indicates that this device generates power, and should have power taken from it and put into batteries.
mech_receiver: Indicates that this device uses power, and should take power from batteries.
```

Example:
```
summon item_frame ~ ~ ~ {Tags:["mech_receiver","custom_machine"],Fixed:1b}
scoreboard players set @e[tag=custom_machine,sort=nearest,limit=1] mech_power 0
```

If you've created a machine correctly, it should automatically be included in the energy grid and batteries/capacitors will start taking or sending power respectively. Now, if you want to generate power, increase the value of the `mech_power` score, and if you want to consume power, decrease the value of the `mech_power` score.

### Adding Batteries/Capacitors

To create a new battery that can send and receive power, summon an entity with the tag `mech_power_storage`. Then, in the devices ticking function run both as and at the entity (recommend 1 time per second), run this code:

```
scoreboard players set $in_0 mech_data <max transfer speed>
scoreboard players set $in_1 mech_data <max machine buffer capacity>
scoreboard players set $in_2 mech_data <max battery capacity>
scoreboard players set $in_3 mech_data <Range: 4,8,12,16,20,24,28, or 32>
scoreboard players set $in_4 mech_data <0 for no effects (particle, sound), 1 for effects>
function mechanization:base/energy/standard

#Alternitive: it your device is supposed to be a battery and draw power from nearby capacitors when they are full, use this command:
#function mechanization:base/energy/battery

#Alternitive: it your device is supposed to be a capacitor and draw energy from nearby batteries when empty, use this command:
#function mechanization:base/energy/capacitor
```

If you plan on making a standalone datapack compatible use Mech's energy system by copying the energy transfer function, make sure to also copy the supporting files. At your convenience, these can be renamed or put in a different location if you also modify the references in the energy transfer function.

```
data/mechanization/predicates/matches_gridid
data/mechanization/predicates/base/valid_transmitter
```

Also make sure to initialize these scoreboards in your load function:

```
scoreboard objectives add mech_data dummy
scoreboard objectives add mech_power dummy
scoreboard objectives add mech_gridid dummy
```

### Portable Energy

Mechanization supports drawing energy from compatible items in the player's hotbar. This is done using these lines of code:

```
scoreboard players set $in_0 mech_data <amount>
function mechanization:base/tools/player_energy/use_energy

execute if score $out_0 mech_data matches 1 run <do something>
```

It is recommended to use built-in portable energy items (like Gadget's Portable Battery), but if you would like to make your own, you can use the following command:
```
give @s carrot_on_a_stick{ mech_battery:{energy:0, max_energy:32000, models:8, base_model:6424900} }

energy: how much power the battery has- usually new batteries have 0
max_energy: how much the battery can hold- the Portable Battery holds 32000 kJ
models: how many models the battery uses to represent charged states. For example, the Portable Battery has a green bar the goes up and down based on energy level that uses 8 models. If you don't have any extra models, set this to 1.
base_model: the stating CMD for extra models. If you don't have extra models, this should be the items normal CMD.
```

# Liquids API
Enabling liquid mechanics for a machine is very complicated. Do not attempt unless you are experienced with minecraft commands. I highly recommend studying/copying examples from Mech machines to get started.

### Enabling Accepting/Receiving Liquids
Extend the function tags `liquid_send` and/or `liquid_accept`. These functions should return if they can accept/send a liquid, how much to accept/send, and what liquid to send.

Send Liquid:
```
temp
```

### Connecting a Machine to Liquid Pipes
1. Extend the function tags `liquid_pipe_can_send` and/or `liquid_pipe_can_accept`. These functions tell liquid pipes if they should pull/push liquid from the side its connected to. Use this code for each machine:
```
execute if entity @s[tag=<custom block>] if score $in_0 mech_data matches 0 run scoreboard players set $out_0 mech_data 1

where $in_0 mech_data is:
0 = up
1 = down
2 = north
3 = south
4 = east
5 = west
(use 0..5 for all sides accept/send)
```

2. When placing a new block that should connect to existing Liquid Pipes, run this code to update adjacent pipes:
```
summon <new block>
execute as @e[<new block>] at @s run function mechanization:machines/machines/liquid_pipe/add_adjacent_pipes
```

# Custom Pneumatic Tubing Interactions

### Extracting
Extend the function tag `custom_item_extraction`, this will be run as your custom block entity. Then, run this code:
```
execute if entity @s[<custom block>] run function ...
 V V V
scoreboard players set $out_0 mech_data 1
# copy for each slot that can be extracted from
execute if data block ~ ~ ~ Items[{Slot:0b}] run data modify storage du:temp list append from block ~ ~ ~ Items[{Slot:0b}]
```

### Inserting
Extend the function tag `custom_item_insertion`, this will be run as your custom block entity. Then, run this code:
```
execute if entity @s[<custom block>] run function ...
 V V V
scoreboard players set $out_0 mech_data 1
scoreboard players set $out_1 mech_data -1

# copy for each valid input slot
execute unless data block ~ ~ ~ Items[{Slot:0b}] run scoreboard players set $out_1 mech_data 0

# Note: you can check the item being inserted with 'if data storage du:temp obj{id:"minecraft:stick"}'- useful if you only want some items being inserted
```

# General Data

There are several event hooks built into mechanization. These are run using a function tag. To extend the event, in your datapack create the folder data/mechanization/tags/functions/[function] and add your functions in that tag (make sure to extend, not overwrite!).

### Function Tags
```
# Tools
wrench_break: triggers when player shift+right clicks a wrench, at the location they are looking. Should be used to safely break machines.
wrench_function: triggers when player right clicks the wrench, at the location they are looking. Should be used to altar a machine setting, i.e. changing modes or rotating.
multimeter_readout: triggers when player right clicks a multimeter, at the location they are looking. By default, prints out how much power the device is currently storing. Can be used to print out additional information.

# Pneumatic Tubing
custom_item_extraction: run `as` and `at` an entity marking a block when a Pneumatic Exit-Point attempts to withdraw an item, allowing defining custom rules.
custom_item_insertion: run `as` and `at` an entity marking a block when a Pneumatic Entry-Point attempts to insert an item, allowing defining custom rules.

# Liquids
liquid_accept: runs as a block when attempting to add a liquid, should return how much liquid was accepted (can be 0)
liquid_send: runs as a block when attempting to extract a liquid, should return a liquid (if available) and the amount available.
liquid_pipe_can_send: runs as a block when a pipe is placed next to it, should return if it can send liquid on the side the pipe is attached to
liquid_pipe_can_accept: runs as a block when a pipe is placed next to it, should return if it can accept liquid on the side the pipe is attached to
```

### Entity Tags
These are entity tags (ie. `tag add @s mech_no_upgrade`). Some states are tracked using tags.
```
mech_transmitter: indicates the devices should send power (ie. is a generator)
mech_receiver: indicates the devices should receive power (ie. is a machine)
mech_power_storage: indicates the device acts like a battery/capacitor and stores power. Enables interactions with the Energy Relay.
mech_no_upgrade: By default, any device can be upgraded. Adding this tag prevents that from happening.
mech_upgraded: Tag added to a machine when a machine upgrade is applied. You should add in effects for each machine when upgraded (works faster, uses less energy, etc).
mech_ender_upgrade: Tag added to a machine when an ender upgrade is applied. Machines require a machine upgrade to already be installed to add an ender upgrade (machine will have this tag AND mech_upgraded). This upgrade is exclusive to Nether Upgrades, so only 1 of the 2 can be added. Ender upgrades typically increase operating efficiency.
mech_nether_upgrade: Tag added to a machine when an nether upgrade is applied. Machines require a machine upgrade to already be installed to add an nether upgrade (machine will have this tag AND mech_upgraded). This upgrade is exclusive to Ender Upgrade, so only 1 of the 2 can be added. Nether upgrades typically increase speed or power, but at a cost.
mech_rotatable: indicates a machine's model should be rotated by a wrench (like the alloy furnace)
```

### Scoreboard Information
```
mech_data: For math, temp variables and other misc data storage. Also used to store an extra bit of data on machines. Could be anything.
mech_power: indicates power level on a machine/generator/battery.
mech_gridid: used internally to specify machines' grid ids. You probably shouldn't mess with this.
mech_usedid: has the Mechanization item ID of whatever the player is holding (if it has an ID), specified by the `mech_itemid:<id>` nbt tag. Very useful for checking what a player is holding instead of using an NBT check.
mech_fluid: used to track how much fluid a machine is holding in its tank. Other objectives may be used if the machine has multiple tanks.
mech_timer: used by machines to track the length of their current operation
```

# Ore Dictonary
As of v3.0, Mechanization's ore dictionary system as been updated to follow the Minecraft Datapacks community guidelines on Ore Dict. You can reference them here: https://mc-datapacks.github.io/en/conventions/common_trait.html

The shorthand is Mech now uses the following format:
`Item.tag.ctc{id:"tin_ingot", from:"mechanization", traits:{ingot:1b, "metal/tin":1b}}`
