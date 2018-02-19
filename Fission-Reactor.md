![Fission Reactor](https://i.imgur.com/zXcoy8d.png?1)

The Fission Reactor is the core of the Fission Reactor multiblock, converting nuclear fuel into heat. Insert fuel rods by right clicking them on the top of the reactor.

Recipe:

![Fission Reactor Recipe](https://i.imgur.com/WLlB6sm.png?1)

## An Introduction to Mechanization Nuclear Physics

![Fission Reactor Multiblock](https://i.imgur.com/Gmf4LQF.png)

The reaction starts in the Fission Reactor block using Nuclear Fuels. Once started, the 'reaction rate' will begin to increase, with the amount of increase based on the type of nuclear fuel (see table). The rate is then multiplied by the fuel grade (how enriched the fuel is from a centrifuge), increasing the reactors 'temperature.' Therefor, the key to managing reactor temperature is to match the fuel grade times the reaction rate with the cooling. If the reactor exceeds 2000 °C, then it will explode! To decrease the reaction rate, use a [control rod](https://github.com/ImCoolYeah105/Mechanization/wiki/Control-Rod).

| Fuel | Rate Increase |
|------|---------------|
| Uranium | 3 |
| Plutonium | 2 |

The next part of the reactor, the cooling unit, is also the most complicated. Each reactor cycle, the reactor will push its heat (equivalent to its temperature) into the 4 block placed around the reactor core. The block then creates some steam depending on how much heat there is, then divides its heat among its neighbors. The block will then cool some of the remaining heat. Every block has a different amount of heat it can transfer into power, divide among its neighbors, and cool off (See table). You will have to play around with them to find the optimal cooling unit.

**Boilers:** High heat -> steam conversion rate, but low exchange rate with neighbors and no cooling.

| Block | Max Heat Exchanged (per neighbor) | Max Heat Converted to Steam |
|-------|-----------------------------------|-----------------------------|
| Iron | 75 | 25 |
| Gold | 50 | 50 |
| Emerald | 25 | 75 |
| Diamond | 10 | 100 |

**Exchangers:** High exchange rate with neighbors and some cooling, but no steam conversion.

| Block | Max Heat Exchanged (per neighbor) | Max Amount Cooled |
|-------|-----------------------------------|-------------------|
| Obsidian | 60 | 40 |
| Coal | 70 | 30 |
| Redstone | 80 | 20 |
| Quartz | 90 | 10 |

**Coolers:** Medium Cooling and Steam conversion, but does not exchange heat with neighbors.

| Block | Max Heat Exchanged (per neighbor) | Max Amount Cooled |
|-------|-----------------------------------|-------------------|
| Snow | 10 | 10 |
| Lapis | 20 | 20 |
| Ice | 30 | 30 |
| Packed Ice | 40 | 40 |