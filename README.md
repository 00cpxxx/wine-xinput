# wine-xinput
XInput for Wine based on DirectInput

In order to exercise DirectInput I implemented support for XInput. XInput is a very simple way to access joysticks that is supposed to superseed DirectInput, but it lacks many features and is more targeted to Xbox controllers. It is closer to the old WinMM joystick API where you need one single function to read joystick data without any setup necessary.

I did not limit the implementation to Microsoft only controllers, it will work with anything that has at least 2 axes and 8 buttons. It is important to understand that the mapping is always based in the Xbox controller so, as in Windows, other controllers will have messed up mapping.

The implementation was tested on Wine 1.9.17 but also applies to 1.8.4 and probably older versions since XInput code is not touched in a long time.

The only games tested were Broforce and Brothers: A Tale of Two Sons. I appreciate more testing and feedback. I only have Xbox 360 controllers so any testing on the original Xbox controllers and Xbox One controllers are welcome.
