.. _`osc backend documentation`: ../midimonster/backends/osc.html
.. _`artnet backend documentation`: ../midimonster/backends/artnet.html

=================================================
Controlling Resolume Avenue using Art-Net via OSC
=================================================

I wanted to control video playback in `Resolume Avenue <https://resolume.com/>`_ from the `DMXControl 3 <https://www.dmxcontrol.de/>`_ lighting control software that I use.  Unfortunately Art-Net control is not available in Avenue (it is included in the much more expensive Arena version), but Avenue *can* be controlled via OSC.

The solution that I've found is to use MIDIMonster to interface between Art-Net and OSC, giving fully configurable control over Resolume Avenue via the lighting software. 


In this example, I'm using

   - MIDIMonster v0.6 on Windows 10
   
In my case, I have both pieces of software installed on the same computer, although there's no reason you couldn't have them on different computers.


Writing the configuration
-------------------------


Configuring Art-Net input
*************************

First we need to tell MIDIMonster which IP address to listen for the Art-Net data. (This is the IP address of the computer that DMXControl is running on, and the standard Art-Net port number 6454).

I'm going to name the Art-Net input "DMXControl".

I'm using universe 3 in the lighting software but DMXControl counts the first universe as 1, whereas MIDIMonster counts the first universe as 0, so universe 3 in DMXControl is universe 2 in MIDIMonster::

	[backend artnet]
	bind = 192.168.0.57 6454
	detect = on

	[artnet dmxcontrol]
	universe = 2

Configuring OSC output
**********************

Because I'm running Resolume on the same computer as DMXControl I am going to output the OSC commands locally using the special localhost (or loopback) IP address: 127.0.0.1 .

I'll need to set a bind control to listen for OSC messages on port 7000, and a destination control to broadcast OSC messages on port 7001.

These are the default ports but can be configured to other ports in the OSC Preferences in Resolume Avenue.

In Resolume, you can find the OSC channel name of any control by going to Shortcuts -> Edit OSC then clicking on the control. In the shortcuts info panel you will see the channel name listed under 'OSC Input', and the OSC Type tag listed underneath.  

I'm going to map one DMX/Art-Net channel to the master opacity control for layer 1, and then map 4 DMX/Art-Net channels to the layer clip triggers.  The opacity is a floating point number from 0.0 to 1.0, and the clip triggers are integers - 0 is off and 1 is on.::

	[osc resolume]
	bind = 127.0.0.1 7001
	destination = 127.0.0.1 7000

	/composition/layers/1/master = f 0.0 1.0
	/composition/layers/1/clips/*/connect = i 0 1

Mapping the channels
********************

Mapping the input channels to the output channels is straightforward::

	[map]
	dmxcontrol.1 > resolume./composition/layers/1/master
	dmxcontrol.{2..5} > resolume./composition/layers/1/clips/{1..4}/connect
	
In summary
----------

Here is what the full configuration code looks like::

	; This configuration maps Art-Net channels to Resolume OSC

	[backend artnet]
	bind = 192.168.0.57 6454
	detect = on

	[artnet dmxcontrol]
	universe = 2

	[osc resolume]
	bind = 127.0.0.1 7001
	destination = 127.0.0.1 7000

	/composition/layers/1/master = f 0.0 1.0
	/composition/layers/1/clips/*/connect = i 0 1

	[map]
	dmxcontrol.1 > resolume./composition/layers/1/master
	dmxcontrol.{2..5} > resolume./composition/layers/1/clips/{1..4}/connect
