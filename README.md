# wine-xinput

A XInput implementation for Wine based on DirectInput. Supports 4 controllers and is supposed to work with non Xbox controllers (but with unknown unpredictable mapping). Distributed in the form of a patch that requires Wine to be compiled from source. Read more below.

### Introduction

In order to exercise DirectInput coding I implemented support for XInput. XInput is a very simple way to access joysticks that is supposed to superseed DirectInput, but it lacks many features and is more targeted to Xbox controllers. It is closer to the old WinMM joystick API where you need one single function to read joystick data without any setup necessary.

I did not limit the implementation to Microsoft only controllers, it will work with anything that has at least 2 axes and 8 buttons. It is important to understand that the mapping is always based in the Xbox controller so, as in Windows, other controllers will have messed up mapping.

This is a patch, it means you are required to apply it in to Wine source and compile Wine with it. If you are not into compiling Wine yourself and prefer using your distro packages you can check https://github.com/KoKuToru/koku-xinput-wine which still requires compilation but uses a different approach that does not touch Wine code.

There are 2 different patches:
* xinput_obiwan.patch - For Wine up to 1.9.17 (deprecated).
* xinput_quigon.patch - For Wine 1.9.19 onwards.

I commited many changes to the official Wine code between 1.9.17 and 1.9.19 so there is no 1.9.18 patch version.

I only have Xbox 360 controllers so any testing on the original Xbox controllers and Xbox One controllers are welcome.

The force feedback emulation is not accurate but it is enough for the games I tested, it may be problematic on other games that require only left or right motor enabled.

It is important to disable Wine (js) joysticks to avoid the same joystick being detected and used twice! To do so get into the wine control panel and then use the joystick applet, in short use this command in the console: wine control joy.cpl

Why the silly patch names? After working so long with "force feedback" in Wine I thought they were proper names.

Why not merging this into official Wine? Because Wine is working towards a different approach to unify joystick input for the different interfaces (WinMM, DirectInput, XInput) using a single HID library. This will simplify the implementations of these DLL's while keeping all ugly code into a single place. So my approach of Xinput from Dinput is not acceptable.

I appreciate more testing and feedback. 

### Games tested so far

* Broforce
* Brothers: A Tale of Two Sons
* Castle Crashers
* Cortex Command
* DiRT 3 (tested by user @Enverex)
* Double Dragon Neon (tested by user @Enverex)
* Duck Tales Remastered
* Fable Anniversary Edition
* Lego Lord of the Rings (tested by @wberrier)
* Lego Marvel Super Heroes (tested by @wberrier)
* Momodora: Reverie Under the Moonlight
* Never Alone
* Shadow Warrior Classic Redux
* Shovel Knight
* Spelunky
* The Adventures of Shuggy
* VVVVVV
* X-Blades

### Games that won't work

* Lego Batman (mixed Dinput+XInput explained in Troubleshooting)
* Lego Star Wars (mixed Dinput+XInput explained in Troubleshooting)
* Sonic & All-Stars Racing Transformed (mixed Dinput+XInput explained in Troubleshooting)

### Gamepads tested so far

* Microsoft XBOX 360 gamepad
* PS3 gamepad (requires remapping using jstest-gtk)
* Generic Twin stick gamepads (require remapping using jstest-gtk)
* Super Nintendo like USB controller (useful only for left stick only games)
* Generic 8 button controller (useful only for left stick only games)

### Troubleshooting

If the game is always looking up or auto selecting axis 3 or Z to anything when you are defining the controls then the game uses a mixed DirectInput + XInput. For now there is no solution to this due to 2 different problems in Wine:

1. Wine does not implement the axes mixing that MS does using the shoulder triggers. In Windows the shoulders act as a single axis, while in Wine they are reported as 2 different axes.
2. Wine does not implement adding the controllers to the PnP devies and append the expected MS "hack" in the controller ID to make the games distinguish between DirectInput and XInput devices.

If the patch is compiled but the game still didn't recognize the controller something is wrong, it could be due to different reasons. For instance:
* Wine DirectInput did not find your controller, check "wine control joy.cpl". Always start the game with the controller previously plugged.
* Some games may install native xinput1\_\*.dll versions and Wine may be trying to load them instead of builtin. Ensure that there are no native xinput DLL's in the system32 folder (no longer necessary from Wine 2.0 onwards).
* Some game controller mapping tool may be hooking the Xinput calls so the calls are not reaching Wine's DLL. The best example is x360ce.
* The game does not use XInput.
