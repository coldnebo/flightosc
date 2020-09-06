# flightosc
using your phone or tablet as a flight controller


## INSTALLATION

You'll need the following pieces of software to make this work:

* [TouchOSC](https://hexler.net/products/touchosc) on your phone or tablet.

On your Windows 10 PC you'll need:

* [TouchOSC Editor](https://hexler.net/products/touchosc#downloads) - edit and upload layouts to the TouchOSC app.
* [TouchOSC Bridge](https://hexler.net/products/touchosc#downloads) - a driver that translates OSC into MIDI.
* [vJoy](http://vjoystick.sourceforge.net/site/) - a virtual game controller driver
* [python3](https://www.python.org/downloads/release/python-385/) - Python3 for Windows.
* (OPTIONAL) [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)
* [git for Windows](https://gitforwindows.org/)
* [midi2vjoy](https://github.com/c0redumb/midi2vjoy) - a pygame script that converts MIDI to vJoy inputs.


_Note: I have a Windows 10 PC -- some of this approach may be adaptable to other platforms, but since my target is MSFS 2020 and DCS World, I'm going to stick with Windows for now._


### Steps

1. Download and install the TouchOSC Editor, Bridge and vJoy from the links above. They are pretty self-explanatory.

	* Open Configure vJoy App and under "Force Feedback", uncheck "Enable Effects".
	* Change "Number of Buttons" from 8 to 30.
	* Restart Windows.
	* Note that "vJoy Device: 1" should be enabled by default.  Adding other devices may confuse MSFS.
	* vJoy Device List when run should display a single number "1" in black and all other device numbers in grey.

2. Then install Python3 for Windows from [the official site](https://www.python.org/downloads/release/python-385/):

	* Download "Windows x86-64 executable installer" from the links on the bottom of the page.
	* Run the installer.
	* Keep "Install launcher for all users (recommended)" checked.
	* Check "Add Python 3.8 to PATH".
	* Click "Install Now"

3. Check Python install.  You should see the following result in a console:
		
		PS C:\Users\lkyra> python -V
		Python 3.8.5

4. Next pick a directory and clone the following projects:

		PS C:\Users\lkyra> mkdir github
		PS C:\Users\lkyra> cd github
		PS C:\Users\lkyra\github> git clone https://github.com/c0redumb/midi2vjoy.git
		PS C:\Users\lkyra\github> git clone https://github.com/coldnebo/flightosc.git

5. Now, install midi2vjoy:

		PS C:\Users\lkyra> cd midi2vjoy
		PS C:\Users\lkyra\midi2vjoy> python setup.py install		

6. Next, run the TouchOSC Bridge and the TouchOSC Editor and then open the following file in the editor:

		C:\Users\lkyra\github\flightosc\simple.touchosc

   * If this has worked, press the "Sync" button in the Editor.
   * Go to TouchOSC on your phone or table and click Layout.
     * Click "Add"
     * Click "Edit", "+" and then enter either the hostname or the IP (I had to enter the IP) of your PC.
     * Click "Done"
     * Click the "Editor Hosts": and click your HOST/IP.
     * "simple" should now show up in Layouts on your phone or tablet. Select it.
     * You'll see a control layout that matches what you see in the Editor... three sliders labeled (throttle, mixture, flaps).  The `touchosc.conf` file in flightosc maps these three sliders to X, Y and Z axes in vJoy.

7. Run midi2vjoy with the included bat file:

		PS C:\Users\lkyra> cd flightosc
		PS C:\Users\lkyra\flightosc> touchosc.bat

8. Test vJoy:

	* Open the "Game Controllers" panel from Windows Settings and select "vJoy Device" and then click "Properties".

	* You should see the X, Y and Z axes move when you move the respective sliders on your phone.

CONGRATS!!! You have successfully configured your phone or tablet as a virtual game controller!

Now go into MSFS 2020 and configure the vJoy device (you may need to look for "custom" configuration) and configure the controls in a way that makes sense to you.  I chose to invert some of the axis so that they visually matched what was happening in the controls... but you can customize it however you want.

Enjoy!


## HISTORY

**2020 Aug 29**: I was minding my own business when one day my throttle controller breaks.  After researching online and trying everything I can think of, I'm pretty sure it's dead. It seems that the firmware/hardware for that device brand sometimes flakes out. So, I have a ticket in to the support site, BUT due to the amazing popularity of Microsoft Flight Simulator 2020, they have a backlog of a few weeks. Also, all the other replacement throttles I might purchase are out-of-stock, again because MSFS 2020 is super popular right now.

So, I needed a replacement, preferably cheap, that I can use in the interim.  I tried the keyboard (of course), but one of the things that is not so obvious about keys is the relative setting of something like a throttle.  For example, if I hold down "increase throttle" for a few seconds, where is the throttle setting?  I don't have a tactile representation of the percentage of throttle I want to set.  Ideally this would have the form of a physical slide switch.  The hunt began and naturally, since all the flight controllers were back-ordered, I turned to another popular kind of device: music controllers.

### Physical Switches

The most promising physical switch I found was [Mine S](https://special-waves.com/) customizable controller at around $299 USD for a basic bundle. This reminds me of Lego Robotics kits, but for audio production. 

Another alternative is the [Korg nanoKONTROL2](https://www.korg.com/us/products/computergear/nanokontrol2/) around $70 USD.  Or, even cheaper, the [Fegoo Easy Control.9](https://www.amazon.com/Fegoo-Control-9-Portable-Slim-Line-Controller/dp/B08CT918Z6/) around $55 USD.

But then I remembered an app I used to use on my phone... TouchOSC.

### Software Switches

[TouchOSC](https://hexler.net/products/touchosc) is a popular implementation for phones and tablets of the [Open Sound Control Protocol](http://opensoundcontrol.org/) and allows virtual sliders to communicate with your computer via OSC/MIDI.

Apparently, I wasn't the only one having these thoughts as I found the [c0redumb/midi2vjoy](https://github.com/c0redumb/midi2vjoy) which connects MIDI outputs to a virtual game controller provided by the [vJoy](http://vjoystick.sourceforge.net/site/) driver.

So, now I discovered a path, from TouchOSC on my iPhone to vJoy to Microsoft Flight Simulator! Victory!!


## FUTURE WORK

There are a lot of steps to get things connected.  In theory, this could be streamlined if a driver was written that could act as an OSC client and implement IGameController in order to be registered as a game device in Windows.  This could avoid having to translate OSC to MIDI and then MIDI to vJoy.

### Refs

* [IGameController Interface](https://docs.microsoft.com/en-us/uwp/api/Windows.Gaming.Input.IGameController?view=winrt-19041) [Windows Dev Center]
* [Guide to OSC Libraries](http://opensoundcontrol.org/guide-osc-libraries) - old
* [Another list of Libraries](https://wiki.thingsandstuff.org/OSC#Programming) - hmmm, Rust? Python/Ruby... etc.  Rust might be best fit for Windows driver development.
