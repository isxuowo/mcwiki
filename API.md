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
2. mech_wrench_function: triggers when player right clicks the wrench. Should be used altar a machine setting, i.e. changing modes or rotating.
* There are two events associated with the multimeter. When used, an entity is spawned inside the block the player is looking at, so commands can be run at the block.
1. mech_meter_readout: triggers when player right clicks a multimeter. By default, prints out how much power the device is currently storing. Can be used to print out additional information.
2. mech_meter_idlock: triggers when player shift+right clicks with a multimeter. By default, locks given machine to provided network id. Really shouldn’t be altered, but can be if necessary.
* World gen event hooks are currently not implemented, but are planned.

# Tips
***
* While your free to create any content in an add on if you want, it’s generally better to avoid overlapping content for performance reasons. Check out the Todo page for a list of planned features.
