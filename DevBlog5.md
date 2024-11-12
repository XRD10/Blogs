# X-Wing VR - Part 2

## Process
We stayed on time with almost all of our planned tasks for the MVP (with 2 tasks in progress and almost done with 1 day left):

![image](https://github.com/user-attachments/assets/2094c252-18ca-4192-bd56-ca477a0f6646)

All the team worked together in the XR lab which helped us to close some discussions and come up with clear goals to achieve until the XR expo.


## Development
Within the MVP milestone we completed the following:
- Found a X-Wing 3D model that we divided into separate parts as needed (joystick and thrust controller) in Blender.
- Created `FlightController` and `VRFlightController` (one of them serves for gamepad / XR controller input, the other converts joystick and thrust position into speed and rotation)
- Added engine sound
- Added shooting
- Added a universe skybox

### 3D object movement into flight controls
Since this project is very time limited, we did not want to waste all of the time on creating many custom components, therefore we took a `XRSlider` and `XRJoystick` controllers from a unity package called **Lets make a VR game** from **[Valem Tutorials](https://www.youtube.com/@ValemTutorials)**. We changed the visuals for our 3D objects that we divided from a previous X Wing model in Blender and created a special `VRFlightController` which takes an input of the thrust/joystick 's position and converts it into a speed and rotation of the spaceship.

The way this works is as follows, the `XRSlider` and `XRJoystick` scripts are emitting unity events with the object's position:

<img width="363" alt="image" src="https://github.com/user-attachments/assets/07f124e2-03e3-4860-a15d-0e1edb5c5eab">

_an example from the `XRJoystick` controller in the editor_


And that is then listened for by the `VRFlightController`'s functions:

<img width="475" alt="image" src="https://github.com/user-attachments/assets/620f6ba9-68a9-4260-8e54-85d7b62c3e74">

As can be seen in the figure above, the `OnThrustChange` function is converting the input into real speed with the help of the `Lerp` function.

THen there is an `Update` function in the `VRFlightController` that takes all the latest input and converts it into a valid speed/rotation of the spaceship.
