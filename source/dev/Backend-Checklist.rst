.. |check| raw:: html

    <input type="checkbox">

Checklist for new backends
==========================

This page is currently a work in progress.

Research
--------

* Find low-level protocol documentation, ideally RFCs or open documentation
* Make sure that implementing the protocol does not violate intellectual property laws
* Layout how the protocol components map onto MIDIMonster terminology (instances, channels, events)
* Figure out what parameters are required to configure the protocol
   * Try to minimize the set of parameters required to get a backend/instance up and running to allow for short configurations
   * Introduce optional parameters to allow users to implement complex use-cases if they want to

Programming
-----------

* Implement a basic skeleton backend (`<backend>.c/h`) and flesh it out with the protocol implementation
* If the new backend requires additional prerequisites, add them to the CI script (but not yet to the documentation)
* If required, add a new target to the backend makefile (but do not yet add the backend to the default build targets)

Documentation
-------------

* Write down the backend documentation
   * Take care to document all required parameters, as well as examples for their use
   * Document the backends channel specification syntax, including examples
* Create an example configuration using the backend

Testing
-------

* Try to run the backend on all platforms it is designed for
* If possible, try to load-test with a mass event generator script

Publish
-------

* Add the new backend to the main README
   * Add to the initial backend overview, including the new backends target operating systems
   * Add to the `backend documentation` section
* If introducing new prerequisites
   * Add the new preprequisites to the main README
