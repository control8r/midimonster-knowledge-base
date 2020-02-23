.. _Artnet: ../midimonster/backends/artnet.html
.. _evdev: ../midimonster/backends/evdev.html
.. _jack: ../midimonster/backends/jack.html
.. _loopback: ../midimonster/backends/loopback.html
.. _lua: ../midimonster/backends/lua.html
.. _maweb: ../midimonster/backends/maweb.html
.. _midi: ../midimonster/backends/midi.html
.. _ola: ../midimonster/backends/ola.html
.. _osc: ../midimonster/backends/osc.html
.. _sacn: ../midimonster/backends/sacn.html
.. _winmidi: ../midimonster/backends/winmidi.html

Backends?
=========

Each protocol supported by MIDIMonster is implemented by a backend, which takes global protocol-specific options and provides instances, which can be configured further.

The configuration is stored in a file with a format very similar to the common INI file format. A section is started by a header in ``[]`` braces, followed by lines of the form option = value.

Lines starting with a semicolon are treated as comments and ignored. Inline comments are not currently supported.

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

Channel mapping
---------------

The ``[map]`` section consists of lines of channel-to-channel assignments, reading like
::

  instance.channel-a < instance.channel-b
  instance.channel-a > instance.channel-b
  instance.channel-c <> instance.channel-d

The first line above maps any event originating from ``instance.channel-b`` to be output
on ``instance.channel-a`` (right-to-left mapping).

The second line makes that mapping a bi-directional mapping, so both of those channels
output eachothers events.

The last line is a shorter way to create a bi-directional mapping.

Multi-channel mapping
---------------------

To make mapping large contiguous sets of channels easier, channel names may contain
expressions of the form ``{<start>..<end>}``, with *start* and *end* being positive integers
delimiting a range of channels. Multiple such expressions may be used in one channel
specification, with the rightmost expression being incremented (or decremented) first for
evaluation.

Both sides of a multi-channel assignment need to have the same number of channels, or one
side must have exactly one channel.

Example multi-channel mapping:
::

  instance-a.channel{1..10} > instance-b.{10..1}

Backend documentation
---------------------

Every backend includes specific documentation, including the global and instance
configuration options, channel specification syntax and any known problems or other
special information.

1. Artnet_
2. evdev_
3. jack_
4. loopback_
5. lua_
6. maweb_
7. midi_
8. ola_
9. osc_
10. sacn_
11. winmidi_