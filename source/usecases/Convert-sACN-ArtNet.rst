.. _`sacn backend documentation`: ../midimonster/backends/sacn.html
.. _`artnet backend documentation`: ../midimonster/backends/artnet.html

=========================
Converting sACN to ArtNet
=========================

Sometimes, you're just stuck with a controller or some fixtures that insist
on speaking only their specific protocol. This use-case is what the MIDIMonster
was originally designed for, acting as a flexible translator between the multitude
of different show control protocols professionals need to deal with.

In this example, I'm using

   - MIDIMonster v0.5 on Windows 7

However, the configurations we're going to create in this example will work on
any system supported by the MIDIMonster, Linux, Windows or OSX.
You could even build a little translation box out of a single-board computer like
the Raspberry Pi to do the translation on its own!

Gathering some information
--------------------------

As both sACN and ArtNet carry lighting control data (DMX) over the network, let's
first gather some information on the network setup.

One very important thing to know is the address range of the network you want to use,
as well as the IP addresses of the target devices. You can run both sACN and ArtNet
in broadcast mode, but sending a lot of universes to all hosts on the network gets
resource intensive very fast.

In this example, I'm using the IPv4 network 10.10.10.0/24, with the MIDIMonster
computer being assigned 10.10.10.1, and an example ArtNet target at 10.10.10.2.

Another thing to note is that sACN by default relies on multicast networking.
The MIDIMonster `sacn` backend is also able to run in both unicast (directly talking
to peers) and broadcast (talking to all hosts on the local network) modes, too,
but using multicast is very convenient for sACN, as we do not need to configure
as many IP addresses.

To use the multicast features of sACN, all the hardware in the path (mainly switches)
need to support the IGMP protocol. Most halfway recent ones do.

Having said all that, we'll start with the core of this article!

Writing the configuration
-------------------------

Let's create a new, empty configuration file and get started on writing down
what we want.

Configuring sACN input
**********************

Let's begin with the sACN input part. As always, having a look at the `sacn backend
documentation`_
can save hours of experimentation ;)

To receive sACN input, we need to specify which IP addresses (and optionally, which ports)
the backend should listen on for incoming data::

	[backend sacn]
	bind = 10.10.10.1

If the sACN data is generated on the computer running the MIDIMonster, we'll need to specify
that line as::

	bind = 10.10.10.1 5568 local

The `local` option will enable the local loopback mode, allowing the MIDIMonster to read multicast
data generated on the local host.

If we wanted to listen for data on multiple interfaces, we could repeat the `bind` option line
once for each additional interface.

After configuring the sACN backend global options, lets move on to configuring our instances.
For the sACN backend, every MIDIMonster instance is one sACN universe, consisting of 512 channels of
DMX data. To receive universes 1 and 2 (universe 0 is reserved in sACN), we simply configure two
instances::

	[sacn sacn1]
	universe = 1

	[sacn sacn2]
	universe = 2

If we had configured multiple interfaces above, we could select the interface for any instance with
the line ::

	interface = 1

This will have that instance receive data from interface `1`. The interface count starts at zero,
which is also the default interface for any instance.

These few lines are all we need for receiving sACN input, so let's move on to the ArtNet output!

Configuring ArtNet output
*************************

The ArtNet backend is very similar to the sACN backend, in that they both transfer lighting control
data over the network and they share some internal architecture. They also have many similar
configuration options, as we can see by checking out the `artnet backend documentation`_.

We're starting out with configuring the interfaces/IP addresses for the data output again::

	[backend artnet]
	bind = 0.0.0.0

As you may see, this IP address is not really valid, but it tells the system that we don't actually
care. This can be useful if, for example, there is only one address on the system. We could also specify
a "real" address here, and we could also specify multiple addresses (and ports) to select between
them for every instance.

To output ArtNet, we again need to configure the instances we want to use with their respective
universes. For ArtNet, universe identifiers start at 1. ::

	[artnet art1]
	universe = 0
	destination = 255.255.255.255

As you can see, to output ArtNet, we need a destination to send the data to. In this case, the broadcast
address is used, which sends the data to all computers connected to the local network.

For the other instance, let's use a proper destination::

	[artnet art2]
	universe = 3
	destination = 10.10.10.2

This sets us up with two universes of ArtNet (universe 0 and universe 3), each providing 512 channels of
output.

Let's see how we can set up the actual translation between them!

Mapping the channels
********************

So far we have told the MIDIMonster where to listen for input and where to send output, but in this
next configuration section we're setting up the mapping from incoming to outgoing channels.

Both the sACN and the ArtNet backends have the same channel specification syntax, which is simply
the channel number (between 1 and 512).

To map all the channels one-to-one, we could use the following lines::

	[map]
	sacn1.{1..512} > art1.{1..512}
	sacn2.{1..512} > art2.{1..512}

This way, all the channels coming in on instance `sacn1` get mapped to the same channels on instance
`art1`, as are the channels from `sacn2` to instance `art2`. We could also cross these, use one instance
as the source for both outputs, assign both to both, split the assignments and all the combinations.

For example, to rearrange the channels so that sACN channel 1 is mapped to ArtNet channel 512, we could
use::

	sacn1.{1..512} > art2.{512..1}
	sacn2.{512..1} > art1.{1..512}

Wrapping up
-----------

In its entirety, a valid configuration for this use case could look like this::

	[backend sacn]
	bind = 10.10.10.1
	
	[sacn sacn1]
	universe = 1

	[sacn sacn2]
	universe = 2

	[backend artnet]
	bind = 0.0.0.0

	[artnet art1]
	universe = 0
	destination = 255.255.255.255

	[artnet art2]
	universe = 3
	destination = 10.10.10.2

	[map]
	sacn1.{1..512} > art1.{1..512}
	sacn2.{1..512} > art2.{1..512}

You can, of course, extend and modify it to your specific liking and use-case.
This configuration will work on any operating system the MIDIMonster supports.

To run the MIDIMonster with this (or any other specific configuration) on Windows,
the simplest way is to just drag-and-drop the configuration file onto the `midimonster.exe`
file in the `download package <https://midimonster.net/download.html>`_.

On other operating systems, most of the time that will work too - otherwise, you can
open a terminal, navigate to the folder containing the MIDIMonster binary and run
`midimonster <path/to/config.file>`.
