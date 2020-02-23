Welcome to the ``MIDIMonster`` Knowledge Base!
======================================


First of all what is **MIDIMonster**?
-------------------------------------
   **MIDIMonster** is a universal control and translation tool that can translate multi-channel absolute-value-based control and/or bus protocols.



What can **MIDIMonster** do?
----------------------------
   You can use **MIDIMonster** for example with an lightconsole and some kind of midi controller, to expand your fader or button count.
      
      But that`s not all! **MIDIMonster** can do much more!
         You can finde some examples and real usecases under ``Usecases``.


Do you need ``HELP``? or found a ``BUG``?
-------------------------------------

   If you encounter a bug or suspect a problem with a a protocol implementation, please
   `open an Issue <https://github.com/cbdevnet/midimonster/issues>`_ or get in touch with us via
   IRC on `Hackint in #midimonster <https://webirc.hackint.org/#irc://irc.hackint.org/#midimonster>`_.

         ***We are happy to hear from you!***


Contributing
------------

   If you like **MIDIMonster** and have found a usecase for it, we would be happy if you could share your experience and knowledge here.

   To do so, go to our `Github repository <https://github.com/control8r/midimonster-knowledge-base>`_ and add some documentation about your solution.



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

   dev/Release-checklist.rst
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
