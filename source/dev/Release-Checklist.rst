MIDIMonster Release Checklist
=============================

This article outlines a set of steps and checks to complete before and after a MIDIMonster release tag.

This page is a work in progress.

Before tagging a release
------------------------

* Development & Build
   * Make sure the software builds under all supported systems
   * Run a recent build through Coverity Scan
   * Check for open issues that should be included in the next release

* Documentation
   * Make sure the README is up to date and reflects the current state of the software
      * Make sure all backends are linked from the main README
   * Check that all backend documentation properly reflects the current state of the backends
      * This includes known bugs & problems
   * Check that the manpage contains up to date and correct information
   * Check that the knowledge base does not make statements that directly contradict the current mainline documentation

* Artifacts
   * Update the debianization on the `debian/master` branch
      * Build a new Debian package using `gbp buildpackage`
