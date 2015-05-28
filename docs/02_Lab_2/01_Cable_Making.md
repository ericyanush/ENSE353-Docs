The first exercise assigned for the second lab was to construct an ethernet network cable.

###Requirements
------------------------

The requirements to construct the cable are:
1. Length of Category 5e or Category 6 cable, long enough to reach switch on end of bench.
2. Two RJ-45 Crimp-on modular connectors.
3. An RJ-45 connector crimper.
4. RJ-45 cable tester.
5. Wire cutters or knife.
	
###Building
----------------

The cable was built to the [TIA-568A cabling standard](http://en.wikipedia.org/wiki/TIA/EIA-568) which specifies the following coloring standard:
With the connector lock facing away from you, and the connector pins at the top the connector:
1. White/Green (Tip)(Left most pin)
2. Green (Ring)
3. White/Orange (Tip)
4. Blue (Ring)
5. White/Blue (Tip)
6. Orange (Ring)
7. White/Brown(Tip)
8. Brown (Ring)
Note: The tip and ring terminology refers to the function of the wire when used in a T-Carrier signaling system.

To construct the cable, the following steps were taken:
1. Strip some the the outer cable jacket from both ends of the cable
2. Locate the jacket stripping string on each end, and use the string to strip approximately 1" of jacketing past the amount stripped in step 1
3. Trim the stripped wire to the point at which the jacket was stripped in step 1.
4. Arrange the individual wires according to the coloring standard listed above.
5. Insert the arranged wires into the modular crimp-on connector, ensuring each wire reaches the end of it's individual channel.
6. Using the connector crimper, crimp the connectors in place.
7. Connect each end of the newly built cable to one component of the cable tester. Verify that each wire is in the correct position and has continuity.

Although the cable could technically be built without performing step 2, following this procedure ensures that any damage to any of the conductors in the cable were not damaged by the tool used to strip the outer jacket.

At this point you have a function ethernet cable that can be connected to the machine [built in lab #1](../Lab_1/Box_Building.html) and to the switch at the end of the bench. You can now use the cable to get [connected to the internet](Connecting_Network.html).