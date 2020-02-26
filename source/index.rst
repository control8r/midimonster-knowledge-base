Welcome to the ``MIDIMonster`` Knowledge Base!
==============================================


First of all: What is **MIDIMonster**?
--------------------------------------
   **MIDIMonster** is a universal control and translation tool for multi-channel absolute-value-based control and/or bus protocols.

   Especially the stage & show control world uses a lot of different protocols of this type, for example to control stage lighting,
   musical instruments and all kinds of effects.

   **MIDIMonster** can also be used as scripting and control environment to generate new and modify existing control data on any of the supported
   protocols.

What can **MIDIMonster** do?
----------------------------
   If you've ever found yourself in the situation of having to connect a device speaking one protocol (for example, a MIDI keyboard) to
   a device speaking another one (such as a lighting desk), **MIDIMonster** can help you!
   
   It provides a simple way to generate and translate events between all the supported protocols, just as you need it.

   This way, you can build and extend your system with the control surfaces you like, configured just the way you like them.

   Thats not all you can do with the **MIDIMonster** though - you can also dynamically generate, program, reroute and modify
   any signals on those protocols. This gives you complete freedom over your show setup.

   For example, if you need parts of your show to run on autopilot, want to switch control of channels between consoles,
   use MIDI from the drummer to beat-sync the light show, give the DJ an easy interface to switch between chasers or
   use your gamepad to control the pitch of your synthesizer - these are all things made possible with the **MIDIMonster** and some
   clever configuration and programming, :)

   **MIDIMonster** can do much more!
   Come find some examples and real usecases under ``Usecases``!

Do you need ``HELP``? or found a ``BUG``?
-----------------------------------------

   If you encounter a bug or suspect a problem with a a protocol implementation, please
   `open an Issue <https://github.com/cbdevnet/midimonster/issues>`_ or get in touch with us via
   IRC on `Hackint in #midimonster <https://webirc.hackint.org/#irc://irc.hackint.org/#midimonster>`_.

         ***We are happy to hear from you!***

Contributing
------------

   If you like **MIDIMonster** and have found a usecase for it, we would be happy if you could share your experience and knowledge here!
   Just create an article in our `Github repository <https://github.com/control8r/midimonster-knowledge-base>`_ and tell us all about your
   adventures and ideas!

.. toctree::
   :caption: General
   :hidden:
   :maxdepth: 1
   :glob:
   
   general/Backends.rst
   general/Installation.rst
   general/*


.. toctree::
   :caption: Backends
   :hidden:
   :maxdepth: 1
   :glob:

   midimonster/backends/artnet.md
   midimonster/backends/evdev.md
   midimonster/backends/jack.md
   midimonster/backends/loopback.md
   midimonster/backends/lua.md
   midimonster/backends/maweb.md
   midimonster/backends/midi.md
   midimonster/backends/ola.md
   midimonster/backends/osc.md
   midimonster/backends/sacn.md

   midimonster/backends/winmidi.md
   midimonster/backends/*


.. toctree::
   :caption: Usecases
   :hidden:
   :maxdepth: 1
   :glob:

   usecases/*


.. toctree::
   :caption: Development
   :hidden:
   :maxdepth: 1
   :glob:

   dev/Release-Checklist.rst
   dev/*


.. toctree::
   :caption: Meta
   :hidden:
   :maxdepth: 1
   :glob:

   Repository <https://github.com/control8r/midimonster-knowledge-base>
   Changelog <https://github.com/control8r/midimonster-knowledge-base/commits/master>
   Legal Notice <https://github.com/control8r/midimonster-knowledge-base>
   Privacy <https://github.com/control8r/midimonster-knowledge-base>
