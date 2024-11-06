# X-Wing VR - Virtual Reality Project

Our Virtual Reality game will allow players to step into the role of Star Wars pilot! Players will be able to fly a X-Wing spaceship through the space, engage in fights against enemy 
TIE interceptors, and experience the thrill of Star Wars-style space combat.

May the force be with you, pilot!

## Development

### Environment

We started with creation of the environment. This was achieved by creating a SkyBox from .exr image, that shows a view of outer space with a planet in the lower part of the frame.

### 3D Models

For the X-Wing cockpit, we combined two models: one had textures but lacked a thrust and rotation controllers, while the other included the controllers but no textures. We separated the controllers from the latter model and applied to the one with textures to complete the setup. This process was done in Blender, following clear and helpful instructions from Kasper's lesson on Blender. The model now has 2 objects that are separated. Thrust controler, that will controll speed and flying joystick, which will allow the pilot to maneuver the X-Wing and control its flight dynamics. The final cockpit view looks like this: 

![Screenshot 2024-11-06 at 3 28 21 pm](https://github.com/user-attachments/assets/cd197ecb-f5d0-4755-932a-5c0ac9ae5ad4)


### Flight Controller
