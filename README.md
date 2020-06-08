# The MIDIMonster Knowledge Base

This repository is where everyone interested in using, documenting, developing
and improving the MIDIMonster can contribute their input.

This includes guides, tips & tricks, how-tos, articles, configuration examples
and wild you could see the MIDIMonster being used for.

## Contributing

Feel free to click the `Edit` button on GitHub to edit files directly from the
browser, or clone the repository using git and commit your changes using your
favourite editor.

Please use the `pull request` functionality to allow the team to check the contribution
before publishing it in the main repository and from there on the MIDIMonster homepage.

## Licensing

Please note that all text and code in this repository is licensed under the BSD 2-Clause
License. To have your contributions be accepted, you need to agree that they are placed
under that same license.

All project logos that may be referenced within this repository are property of the
respective project.

## Building

To see how your changes would look before committing them, you can use the same process
we use on the server to build the knowledge base website.

### Clone the repository

First, clone the repository with all subrepositories using the command

```
git clone --recurse-submodules https://github.com/control8r/midimonster-knowledge-base
```

### Install the prerequisites

To build the website, some additional tools are required. When using Debian or a derivative
distribution, you can install the following packages using `apt-get`:

* python3-sphinx
* python3-recommonmark
* python3-sphinx-rtd-theme
* python3-pip

There are a few dependencies without an coresponding package. (If you installed all dependencies without any package via pip you're good to go.)

```shell
$ virtualenv venv --python=python3.7
$ source venv/bin/activate
$ pip install -r requirements-apt.txt
```

If your distribution does not provide these or equivalent packages, you can also install
the packages using `pip` by running

```shell
$ virtualenv venv --python=python3.7
$ source venv/bin/activate
$ pip install -r requirements.txt
```

### Build the webpage

To actually generate the output web pages with your new changes, run

```shell
$ source venv/bin/activate
$ make html
```

If you used the distribution packages, a simple `make clean html` should suffice.

The output will be placed in the `build/html` directory and can be viewed from there
with any web browser.
