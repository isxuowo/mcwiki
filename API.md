Mechanization is modularized into a set of event calls and scoreboard information. As such, it is very easy to develop add-ons that use Mechanization's features to create a seamless experience. This page documents everything you might find useful in creating an add-on.

Note: Mechanization depends on Datapack Utils for things like crafting, smelting, custom tools and world generation. Since Mech depends on DU, you can use all of its API in your project as well. You can get more information over on its [wiki](https://github.com/ImCoolYeah105/Datapack-Utilities/wiki).

# Energy API
***

Mechanization's primary feature is the energy grid. To create a device that interacts with the grid, set an entity's 'mech_power' score to 0. Then, give it one of these tags:
* mech_transmitter: Indicates that this device generates power, and should have power taken from it and put into batteries.
* mech_reciever: Indicates that this device uses power, and should take power from batteries.

Once a machine has one of those tags, it is automatically included in the grid. Further interaction is based on the mech_power scoreboard value. Generators should increase this value to reflect energy generation, and machines should decrease this value to reflect energy consumption.

To create a new battery that can send and receive power, summon an entity with the tag `mech_power_storage`. Then, run the following code segment both `as` and `at` an entity:
```
scoreboard players set in_0 mech_data <max transfer speed>
scoreboard players set in_1 mech_data <max machine buffer capacity>
scoreboard players set in_2 mech_data <max battery capacity>
scoreboard players set in_3 mech_data <Range: 12/16/24>
function mechanization:base/energy/battery
```

### Portable Energy

Mechanization supports drawing energy from compatible items in the player's hotbar. This is done using these lines of code:

    scoreboard players set in_0 mech_data 8 //This is how much power to draw
    function mechanization:base/tools/player_energy/use_energy

This will attempt to draw power from the player. It will then set the score 'out_0 mech_data' to 0 or 1, where 0 is failed to draw energy an 1 is succeed in drawing energy.

It is recommended to use built-in portable energy items (like Gadget's Portable Battery), but if you would like to make your own then give it the nbt tag 'Energy:1' (Energy must always be above 1). This item can then be charged at the Charging Station, or you can add your own way to charge it.

# Events
***
There are several event hooks built into mechanization. These are run using a function tag. To extend the event, in your datapack create the folder data/mechanization/tags/functions/[function] and add your functions in that tag (make sure to extend, not overwrite!).

### Tools
* wrench_break: triggers when player shift+right clicks a wrench, at the location they are looking. Should be used to safely break machines.
* wrench_function: triggers when player right clicks the wrench, at the location they are looking. Should be used to altar a machine setting, i.e. changing modes or rotating.
* multimeter_readout: triggers when player right clicks a multimeter, at the location they are looking. By default, prints out how much power the device is currently storing. Can be used to print out additional information.

### Machine Tags
* mech_no_upgrade: By default, any device can be upgraded. Adding this tag prevents that from happening.
* mech_upgraded: Tag added to a machine when a machine upgrade is applied. You should add in effects for each machine when upgraded (works faster, uses less energy, etc).
* mech_ender_upgrade: Tag added to a machine when an ender upgrade is applied. Machines require a machine upgrade to already be installed to add an ender upgrade (machine will have this tag AND mech_upgraded). This upgrade is exclusive to Nether Upgrades, so only 1 of the 2 can be added. Ender upgrades typically increase operating efficiency.
* mech_nether_upgrade: Tag added to a machine when an nether upgrade is applied. Machines require a machine upgrade to already be installed to add an nether upgrade (machine will have this tag AND mech_upgraded). This upgrade is exclusive to Ender Upgrade, so only 1 of the 2 can be added. Nether upgrades typically increase speed or power, but at a cost.

# Scoreboard Information
***
* mech_data: For math, temp variables and other misc data storage. Also used to store an extra bit of data on machines. Could be anything.
* mech_power: indicates power level on a machine/generator/battery.
* mech_x, mech_y, mech_z: used when position or rotation cords need to be stored.
* mech_gridid: used internally to specify machines' grid ids. You probably shouldn't mess with this.
* mech_usedid: used internally to indicate what item id an item had when used.

# Ore Dictonary
***
The purpose of the ore dictionary is essentially the same as in modded Minecraft- to improve compatibility by having an easy way of identifying certain items in a predictable manner. Generally, this isn't done by interfacing with the API, but rather is done on your own.

Adding items to ore dict: inside the tag:{} nbt list, add the following string array tag: OreDict:[]. Then, add any ore dictionary entries to the array. For example, OreDict:["ingotSteel"]. Multiple entries can be specified. Naming conventions should follow the normal conventions for modded Minecraft (why reinvent the wheel).

Using ore dict items in recipes: Just test for the needed ore dict entry in the array, for example in dropper crafting:

    execute if block ~ ~ ~ dropper{Items:[{Slot:0b,Count:1b, tag:{OreDict:["ingotSteel"]} }]}

Following these conventions will allow easy cross-datapack compatibility with recipes, and will simplify recipe creation in the long run.

# Tips
***
* While your free to create any content in an add on if you want, it’s generally better to avoid overlapping content for performance reasons. Check out the Todo page for a list of planned features.
* Need an API hook or think that something should be added to the API? Please post a suggestion in the issue tracker (but make sure it isn't already on the [todo list](https://github.com/ImCoolYeah105/Mechanization/wiki/Official-TODO-list)).
