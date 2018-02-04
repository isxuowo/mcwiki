Mechanization is modularized into a set of event calls and scoreboard information. As such, it is very easy to develop add-ons that use Mechanization's features to create a seamless experience. This page documents everything you might find useful in creating an add-on.

*Important notice: this page is being developed along side the 1.13 release of Mechanization, and therefor most of the information is not applicable as of right now. Once released this information will be up-to-date and valid.

You can download the most recent 1.13 snapshot (Version 0.3.0) build [here](https://www.dropbox.com/s/kppubwx3bnxnarc/MechanizationSnapshot.zip?dl=1).

And the resource pack [here](https://www.dropbox.com/s/030kabg3pylzk29/MechanizationResourcePack.zip?dl=1), (last updated in Version 0.3.0).

Installation: Put Mechanization_1.13_Snapshot.zip into to /datapacks folder for the world you want to install it for. Enter the world and run the commands /reload and /function mechanization:start

# Energy API
***

Mechanization's primary feature is the energy grid. To create a device that interacts with the grid, give an entity one of these tags:
* mech_transmitter: Indicates that this device generates power, and should have power taken from it and put into batteries.
* mech_reciever: Indicates that this device uses power, and should take power from batteries.

Once a machine has one of those tags, it is automatically included in the grid. Further interaction is based on the mech_power scoreboard value (which should be initialized to 0, not done automatically). Generators should increase this value to reflect energy generation, and machines should decrease this value to reflect energy consumption.

Note: it is not currently possible to create custom batteries without extensive scripting involved.

# Events
***
There are several event hooks built into mechanization. These are generally provided using an entity with a tag at the location of the event.
* mech_wrench_break: triggers when player shift+right clicks a wrench, at the location they are looking. Should be used to safely break machines.
* mech_wrench_function: triggers when player right clicks the wrench, at the location they are looking. Should be used to altar a machine setting, i.e. changing modes or rotating.
* mech_meter_readout: triggers when player right clicks a multimeter, at the location they are looking. By default, prints out how much power the device is currently storing. Can be used to print out additional information.
* mech_activegen: indicates a chunk is ready to custom generation.
* mech_sneaking: indicates a player is currently sneaking
* mech_placeobject: triggers when a player places a player head, at the location they are looking. Used internally to place custom machines/blocks.
* mech_right_click: give an item the nbt data tag.ClickDetect:1 to turn on right click detection while holding that item. This tag is then applied to anyone who right clicks (imperfect detection, may fail when players looks around very fast).
* mech_upgraded: added to machine when a machine upgrade is applied. You should add in effects for each machine when upgraded (works faster, uses less energy, etc).

# Scoreboard Information
***
* mech_timer: used when anything requires time. Has 2 fake players "timer" and "timer100" timer is a 20 tick clock, timer250 is a 100 tick clock. You can use them to put machines on a delay.
* mech_data: For math, temp variables and other misc data storage. Also used to store an extra bit of data on machines. Could be anything.
* mech_power: indicates power level on a machine/generator/battery.
* mech_uuid: used whenever something needs a unique id. Uses the fake player incrID. You should always get a new id from this fake player, and increment it by 1 when you do.
* mech_x, mech_y, mech_z: used when position or rotation cords need to be stored.
* mech_gridid: used internally to specify machines' grid ids. You probably shouldn't mess with this.
* mech_usedid: used internally to indicate what damage an item had when used. Paired with the 'mech_right_clicked' tag.

# Ore Dictonary
***
The purpose of the ore dictionary is essentially the same as in modded Minecraft- to improve compatibility by having an easy way of identifying certain items in a predictable manner. Generally, this isn't done by interfacing with the API, but rather is done on your own.

Adding items to ore dict: inside the tag:{} nbt list, add the following string array tag: OreDict:[]. Then, add any ore dictionary entries to the array. For example, OreDict:["ingotSteel"]. Multiple entries can be specified. Naming conventions should follow the normal conventions for modded Minecraft (why reinvent the wheel).

Using ore dict items in recipes: Just test for the needed ore dict entry in the array (example coming when nbt data support is added to custom recipes).

Following these conventions will allow easy cross-datapack compatibility with recipes, and will simplify recipe creation in the long run.

# Additional Information
***
* The world spawn is set to x=0, z=0. This way the spawn chunk cords are always known.

# Tips
***
* While your free to create any content in an add on if you want, it’s generally better to avoid overlapping content for performance reasons. Check out the Todo page for a list of planned features.
* Need an API hook or think that something should be added to the API? Please post a suggestion in the issue tracker (but make sure it isn't already on the [todo list](https://github.com/ImCoolYeah105/Mechanization/wiki/Official-TODO-list)).
