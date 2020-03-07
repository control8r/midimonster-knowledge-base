.. _ArtNet: ../midimonster/backends/artnet.html
.. _`evdev Input Devices`: ../midimonster/backends/evdev.html
.. _JACK: ../midimonster/backends/jack.html
.. _Loopback: ../midimonster/backends/loopback.html
.. _Lua programming: ../midimonster/backends/lua.html
.. _`MA Lighting GrandMA2`: ../midimonster/backends/maweb.html
.. _`ALSA MIDI`: ../midimonster/backends/midi.html
.. _OLA: ../midimonster/backends/ola.html
.. _OSC: ../midimonster/backends/osc.html
.. _sACN: ../midimonster/backends/sacn.html
.. _OpenPixelControl: ../midimonster/backends/openpixelcontrol.html
.. _`Windows MIDI`: ../midimonster/backends/winmidi.html
.. _`Python programming`: ../midimonster/backends/python.html

What are backends?
==================

Each protocol supported by **MIDIMonster** is provided by a backend, which comes as a shared
library file (`.so` on Linux/OSX, `.dll` on Windows). This allows the developers to extend the
project with new protocols without having to touch core code all the time.

Backends take global protocol-specific options and provide instances, which can be configured further.
These instances then provide the channels you can map to eachother to translate values.

For example, the ArtNet backend provides global options for the adresses it is supposed to listen on and
provides instances for each universe it receives or sends.

Example configuration files may be found at our Github at `/configs <https://github.com/cbdevnet/midimonster/tree/master/configs>`_.

Backend and instance configuration
----------------------------------

A configuration section may either be a *backend configuration* section, started by
``[backend <backend-name>]``, an *instance configuration* section, started by
``[<backend-name> <instance-name>]`` or a *mapping* section started by ``[map]``.

Backends document their global options in their [backend documentation](#backend-documentation).
Some backends may not require global configuration, in which case the configuration
section for that particular backend can be omitted.

To make an instance available for mapping channels, it requires at least the
``[<backend-name> <instance-name>]`` configuration stanza. Most backends require
additional configuration for their instances.

Backend and instance configuration options can also be overridden via command line
arguments using the syntax ``-b <backend>.<option>=<value>`` for backend options
and ``-i <instance>.<option>=<value>`` for instance options. These overrides
are applied when the backend/instance is first mentioned in the configuration file.

Backend documentation
---------------------

Every backend includes specific documentation, including the global and instance
configuration options, channel specification syntax and any known problems or other
special information.

Network Protocols:
   * ArtNet_
   * sACN_
   * OSC_
   * OpenPixelControl_

Interfaces to other controllers:
   * `MA Lighting GrandMA2`_
   * OLA_

Hardware Interfaces:
   * `evdev Input Devices`_
   * JACK_
   * `ALSA MIDI`_
   * `Windows MIDI`_

Programming & Simplicity:
   * `Lua programming`_
   * `Python programming`_
   * Loopback_
