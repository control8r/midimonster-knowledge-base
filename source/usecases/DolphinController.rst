==================================================
Using the MIDIMonster as a Dolphin game controller
==================================================

In this article, I'm going to show you how to play console games (via an emulator) with the MIDIMonster.
This example can be extended to create new controller types, input methods or to script game input using the
programming backends to play automatically or semi-automatically.

In this example, I'm using

   - MIDIMonster v0.5 on Debian 10.4
   - Dolphin 5.0 from the Debian repositories
   - A Launch Control MIDI controller as input

Setting up the emulator
-----------------------

If you're not familiar with it, `Dolphin <https://dolphin-emu.org/>`_ is an open source emulator that allows
you to play GameCube and Wii games on your computer. I'm not going to go into the legal situation of playing games
via an emulator, but suffice to say that you should always make sure that you actually own the game you're playing.

Dolphin allows you to use any gamepad, joystick, mouse or connected Wii Controller to play your favourite games.

We're hooking into that using the MIDIMonster `evdev backend <https://github.com/cbdevnet/midimonster/blob/master/backends/evdev.md>`_,
which can not only use mouse, keyboard and gamepad input to control any other protocol, but can also act as a
*virtual input device*, making it possible to use it to create a virtual gamepad (or mouse, keyboard, etc. for that
matter), controlled by any other protocol and scripting language supported by the MIDIMonster.

This functionality is only available on Linux, as Windows and OSX do not support that specific feature.

I'm using my standard every-day computer, running a Debian `stable` installation. That system allows me to install
Dolphin using the command ::

    #> apt-get install dolphin-emu

You will need to run this from a root shell, or just add `sudo` in front of the command. Alternatively, you
can also download the latest version directly from the `Dolphin project homepage <https://dolphin-emu.org/>`_.

Writing the configuration
-------------------------

Let's start with an fresh, empty configuration file. First, we're going to configure the control input into
the MIDIMonster. In this case, I want to use my Launch Control MIDI controller. On Linux, I can use the `midi
backend <https://github.com/cbdevnet/midimonster/blob/master/backends/midi.md>`_ to get input from and send output
to MIDI devices.

Now, because I use this controller a lot, I have customized its internal configuration so that the 16 rotaries
control output MIDI Control Changes numbered 0 through 15, and the eight pads below output MIDI Note 0 through 7.
If you're using another backend for input, or want to use a different controller, you will have to find out
what MIDIMonster channels that input comes in on.

Some backends, such as the `midi` backend, offer an option that lets you see any incoming data on the console,
which helps greatly with finding that out. Have a look at the `documentation file <https://github.com/cbdevnet/midimonster#backend-documentation>`_
of the backend you're intending to use to find out more.

So, to get input from my Launch Control into the MIDIMonster, I'm creating an instance that will read from the
controller using just two lines of configuration::

    [midi lc]
    read = Launch Control


Note that I omitted the backend configuration section for the `midi` backend, because I'm fine with using the
default values for that so far.

The next step is to create another instance for the controller output, this time using the `evdev` backend.
Again the `backend documentation file <https://github.com/cbdevnet/midimonster/blob/master/backends/evdev.md>`_
gives a lot of useful hints for configuring this.

We'll start with ::

    [evdev control]
    output = MIDIMonster

This creates an `evdev` instance named "control", and enables it for event output mode, thus creating a new
virtual input device in the system. From now on, we will need to run the MIDIMonster with superuser (`root`)
privileges, because creating virtual input devices such as keyboards requires administrative privileges.

For simple games, we can get by with our virtual controller just having 2 main axes (X and Y for the main
control stick) and a few buttons. Of course you can extend this configuration as far as you like, to match
any game controller real or imaginable. You could, for example, use the `evtest` tool to get all the information
about which axes and keys are used by your favourite controller, and build a scriptable version of it using
the MIDIMonster.

To be able to output absolute axis information correctly, our `evdev` instance needs information about the range
those axes support. We supply this information using the next two lines of configuration ::

    axis.ABS_X = 34300 0 65536 255 4095
    axis.ABS_Y = 34300 0 65536 255 4095

After this, our basic two-axis, two-button controller configuration is almost finished, we just have to set
up the translation. This section is where we tell the MIDIMonster what values coming in on the MIDI input instance
to translate to which channels on the evdev output instance::


    [map]
    lc.ch0.cc0 > controller.EV_ABS.ABS_X
    lc.ch0.cc1 > controller.EV_ABS.ABS_Y
    lc.ch0.note0 > controller.EV_KEY.BTN_A
    lc.ch0.note1 > controller.EV_KEY.BTN_B

With this section, I can now control the X and Y axis of the main control stick of our virtual controller using
the first two rotary controls on my Launch Control. The first two pads are similarly mapped to the "A" and "B"
buttons.

In its entirety, the configuration now looks like this::

    [midi lc]
    read = Launch Control
    
    [evdev controller]
    output = MIDIMonster
    axis.ABS_X = 34300 0 65536 255 4095
    axis.ABS_Y = 34300 0 65536 255 4095
    
    [map]
    lc.ch0.cc0 > controller.EV_ABS.ABS_X
    lc.ch0.cc1 > controller.EV_ABS.ABS_Y
    lc.ch0.note0 > controller.EV_KEY.BTN_A
    lc.ch0.note1 > controller.EV_KEY.BTN_B

Saving the configuration and running it as the root user, for example with the command ::

    #> sudo ./midimonster gamecube.cfg

will connect to our input device, create the new virtual output device and start translating!

Configuring dolphin
-------------------

Let's move on to getting Dolphin to use our new controller to play some games!

To do that, we will start Dolphin and click the "Controllers" Icon in the top icon bar. Alternatively,
we can also open the options menu and click on "Controller settings", which will bring up the same window.
Now we select "Port 1" to be a "Standard controller" and hit "Configure". This will bring us to a window
where we can configure the controller for player 1.

In this new Window, we can make individual assignments for each control. Click "Refresh" next to
the device dropdown to update the list of available input devices and then select the "MIDIMonster" device
from the dropdown list.

To configure the controller for use with Dolphin games, we can now click the button next to "A" and then press
the corresponding key on the MIDI controller. This will update Dolphins internal mapping. We can to this again
for the "B" button and the main control stick movement axes.

Once done, we can optionally save the profile we created under a name, so we can load it up again later.

Starting up an emulated game, we can now control player 1 using the Launch Control!

