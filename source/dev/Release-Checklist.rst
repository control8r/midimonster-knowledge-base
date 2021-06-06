.. |check| raw:: html

    <input type="checkbox">

MIDIMonster Release Checklist
=============================

This article outlines a set of steps and checks to complete before and after a MIDIMonster release tag.

This page is a work in progress.

Before tagging a release
------------------------

* Development & Build
   * |check| Make sure the software builds under all supported systems
   * |check| Run a recent build through Coverity Scan
   * |check| Check for open issues that should be included in the next release
   * |check| Check that all hardcoded version numbers are correct for the new release
      * This mainly concerns `midimonster.h` and `midimonster.rc`
   * |check| Run a complete build with the `DEBUG` option enabled
   * |check| Ensure that all backends are built without the `DEBUG` define

* Documentation
   * |check| Make sure the README is up to date and reflects the current state of the software
      * |check| Make sure all backends are linked from the main README
   * |check| Check that all backend documentation properly reflects the current state of the backends
   * |check| If something in the backend documentation changes, update the midimonster submodule in the knowledge-base repository.
      * |check| This includes known bugs & problems
   * |check| Check that the manpage contains up to date and correct information
   * |check| Check that the knowledge base does not make statements that directly contradict the current mainline documentation

After tagging a release
-----------------------

* Artifacts
   * |check| Update the debianization on the `debian/master` branch
      * |check| Build a new Debian package using `gbp buildpackage`
   * |check| Build binary packages for the target distributions
      * |check| Build the manual backends where necessary

* Distribution
   * |check| Upload artifacts to the homepage and update the downloads section
   * |check| Create a new release section on GitHub

* Publicity
   * |check| Announce the new release on the Homepage and appropriate social media channels
