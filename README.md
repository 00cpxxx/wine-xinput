# wine-xinput
XInput for Wine based on DirectInput

In order to exercise DirectInput coding I implemented support for XInput. XInput is a very simple way to access joysticks that is supposed to superseed DirectInput, but it lacks many features and is more targeted to Xbox controllers. It is closer to the old WinMM joystick API where you need one single function to read joystick data without any setup necessary.

I did not limit the implementation to Microsoft only controllers, it will work with anything that has at least 2 axes and 8 buttons. It is important to understand that the mapping is always based in the Xbox controller so, as in Windows, other controllers will have messed up mapping.

There are 2 different patches:
* xinput_obiwan.patch - For Wine up to 1.9.17.
* xinput_quigon.patch - For Wine 1.9.19 onwards.

I commited many changes to the official Wine code between 1.9.17 and 1.9.19 so there is no 1.9.18 patch version.

I only have Xbox 360 controllers so any testing on the original Xbox controllers and Xbox One controllers are welcome.

The force feedback emulation is not accurate but it is enough for the games I tested, it may be problematic on other games that require only left or right motor enabled.

It is important to disable Wine (js) joysticks to avoid the same joystick being detected and used twice! To do so get into the wine control panel and then use the joystick applet, in short use this command in the console: wine control joy.cpl

Why the silly patch names? After working so long with "force feedback" in Wine I thought they were proper names.

Why not merging this into official Wine? Because Wine is working towards a different approach to unify joystick input for the different interfaces (WinMM, DirectInput, XInput) using a single HID library. This will simplify the implementations of these DLL's while keeping all ugly code into a single place. So my approach of Xinput from Dinput is not acceptable.

Games tested so far:
* Broforce
* Brothers: A Tale of Two Sons
* Momodora: Reverie Under the Moonlight

I appreciate more testing and feedback. 
