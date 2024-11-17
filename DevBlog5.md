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

Then there is an `Update` function in the `VRFlightController` that takes all the latest input and converts it into a valid speed/rotation of the spaceship.

### Customization of the `XRJoystick` script
Using pre-created scripts from an imported unity package is a huge help in terms of development speed, however it often brings a few issues along, as there usually is some desired functionality that is not included and then you have to change it manually, this can be a tough process if the used script is complex and hard to understand. Luckily for us, we only needed few minor changes that we were able to implement after diving into the code for some time:

#### Disabling rotation of the joystick alongside its y axis
While testing, we noticed that at certain point, the joystick started to rotate in a weird way alongside the y axis. Since our joystick shape is not straight, it was a distractive element and we wanted to get rid of it. We found out, that there was a function that was setting the joystick's rotation called `SetHandleAngle` and all that needed to be done was to set the y angle update to 0: 

![image](https://github.com/user-attachments/assets/64675979-d6f7-4211-ae83-2246eaa7bf48)

#### Hiding an XR controller/hand on joystick grab
To bring more realism into our game we used animated hands instead of controllers in the scene from the previously mentioned **Lets make a VR game** package. We noticed that when grabbing the controller, it would snap to a wrist position of the hand instead of a palm. While trying to find a solution to this issue we realized it would probably be best if we hid the hand while grabbing the joystick (a solution many VR games apply) so that it isclear that the player is controlling it.

We added both hand as GameObject to the `XRJoystick` script and made a simple logic in the `StartGrab` and `EndGrab` functions, where we check the name of the Interactor grabbin the object and since our custom naming is following a clear structure of `Right_InteractorName` and `Left_InteractorName` for both hands we only check for the beggining of the string and based on that we disable/enable the corresponding hand object:

<img width="413" alt="image" src="https://github.com/user-attachments/assets/7ab4c50a-241a-4dce-b61b-d62de9dd972d">

### Imperial Star Destroyer and implementation of LOD
The implementation of LOD system allows our Star Destroyer model to maintain optimal performance across various viewing distances. We have removed some groups that are not important for larger distances in Blender. The resulting models were then imported into Unity, where we set up a StarDestroyer prefab with LOD group components with three distinct detail levels. The technical specifications of each LOD level are in the figure below.

![Screenshot 2024-11-15 at 9 55 38 pm](https://github.com/user-attachments/assets/ab40b680-ab11-4542-a9b8-30b7a2677434)

This LOD implementation provides significant performance optimization:
- 80% polygon reduction at medium distances (LOD1)
- 97% polygon reduction at long distances (LOD2)
- Automatic transition between detail levels based on viewing distance


