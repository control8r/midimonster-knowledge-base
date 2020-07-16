.. |check| raw:: html

    <input type="checkbox">

Checklist for new backends [WIP]
================================

This page is currently a work in progress.

Research
--------

* |check| Find low-level protocol documentation, ideally RFCs or open documentation
* |check| Make sure that implementing the protocol does not violate intellectual property laws
* |check| Layout how the protocol components map onto MIDIMonster terminology (instances, channels, events)
* |check| Figure out what parameters are required to configure the protocol
   * |check| Try to minimize the set of parameters required to get a backend/instance up and running to allow for short configurations
   * |check| Introduce optional parameters to allow users to implement complex use-cases if they want to

Programming
-----------

* |check| Implement a basic skeleton backend (`<backend>.c/h`) and flesh it out with the protocol implementation
* |check| If the new backend requires additional prerequisites, add them to the CI script (but not yet to the documentation)
* |check| If required, add a new target to the backend makefile (but do not yet add the backend to the default build targets)

Documentation
-------------

* |check| Write down the backend documentation
   * |check| Take care to document all required parameters, as well as examples for their use
   * |check| Document the backends channel specification syntax, including examples
* |check| Create an example configuration using the backend

Testing
-------

* |check| Try to run the backend on all platforms it is designed for
* |check| If possible, try to load-test with a mass event generator script

Publish
-------

* |check| Add the new backend to the main README
   * |check| Add to the initial backend overview, including the new backends target operating systems
   * |check| Add to the `backend documentation` section
* |check| If introducing new prerequisites
   * |check| Add the new preprequisites to the main README
