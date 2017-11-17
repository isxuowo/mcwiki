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

