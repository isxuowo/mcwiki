Mechanization is modularized into a set of event calls and scoreboard information. As such, it is very easy to develop add-ons that use Mechanization's features to create a seamless experience. This page documents everything you might find useful in creating an add-on.

*Important notice: this page is being developed along side the 1.13 release of Mechanization, and therefor most of the information is not applicable as of right now. Once released this information will be up-to-date and valid.

# Energy API
***

Mechanization's primary feature is the energy grid. To create a device that interacts with the grid, give an entity one of these tags:
* mech_transmitter: Indicates that this device generates power, and should have power taken from it and put into batteries.
* mech_reciever: Indicates that this device uses power, and should take power from batteries.

Once a machine has one of those tags, it is automatically included in the grid. Further interaction is based on the mech_power scoreboard value. Generators should increase this value to reflect energy generation, and machines should decrease this value to reflect energy consumption. While initializing the mech_power score for a device is not mandatory, it is recommended to help prevent issues with the Grid ID system.

Note: it is not currently possible to create custom batteries without extensive scripting involved.

# Events
***
There are several event hooks built into mechanization. These are generally provided using an entity with a tag at the location of the event.
* There are two events associated with the wrench. When used, an entity is spawned inside the block the player is looking at, so commands can be run at the block.
1. mech_wrench_break: triggers when player shift+right clicks a wrench. Should be used to safely break the machine.
2. mech_wrench_function: triggers when player right clicks the wrench. Should be used to altar a machine setting, i.e. changing modes or rotating.
* There are two events associated with the multimeter. When used, an entity is spawned inside the block the player is looking at, so commands can be run at the block.
1. mech_meter_readout: triggers when player right clicks a multimeter. By default, prints out how much power the device is currently storing. Can be used to print out additional information.
2. mech_meter_idlock: triggers when player shift+right clicks with a multimeter. By default, locks given machine to provided network id. Really shouldn’t be altered, but can be if necessary.
* World gen event hooks are currently not implemented, but are planned.

# Scoreboard Information
***
* mech_timer: used when anything requires time. Has 2 fake players "timer" and "timer250" timer is a 20 tick clock, timer250 is a 250 tick clock. You can use them to put machines on a delay.
* mech_data: used for storing constants and for math. Also used to store an extra bit of data on machines. Could be anything.
* mech_power: indicates power level on a machine/generator/battery.
* mech_uuid: used whenever something needs a unique id. Uses the fake player incrID. You should always get a new id from this fake player, and increment it by 1 when you do.
* mech_x, mech_y, mech_z: used when position or rotation cords need to be stored.

# Helper Functions
***
These function files are in the API. Generally they require scoreboard inputs using fake players. These inputs are specified in the individual function file header.
* function mechanization:base/math/sine - Calculates sine given angle in degrees.
* function mechanization:base/math/cosine - Calculates cosine given angle in degrees.
* function mechanization:base/math/tangent- Calculates tangent given angle in degrees.
* function mechanization:base/math/power- Raised number to specified power.
* function mechanization:base/raytrace/start_ray- Spawns an instant raycast at entity.
* function mechanization:base/raytrace/start_dynamic_ray- Spawns a dynamic raycast at entity.

# Additional Information
***
* The world spawn is set to x=0,z=0. This way the spawn chunk cords are always known.

# Tips
***
* While your free to create any content in an add on if you want, it’s generally better to avoid overlapping content for performance reasons. Check out the Todo page for a list of planned features.
* Need an API hook or think that something should be added to the API? Please post a suggestion in the issue tracker (but make sure it isn't already on the [todo list](https://github.com/ImCoolYeah105/Mechanization/wiki/Official-TODO-list)).
