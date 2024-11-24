# X-Wing VR - Part 3

## Process
The final part of the VR project was really strong for our team as we were motivated from how the project has been developing we stayed very productive throughout the whole period and managed to finish most of the tasks, as can be seen in the figure below (there is one open task left and at the time of writing this post it is mostly implemented and not far from being reviewed, so it is on track):

![image](https://github.com/user-attachments/assets/f7585943-6279-4486-b94c-1de17cdc373a)

In this final period of the project, the team worked mostly online, but with a strong communication we managed to stay synchronized and effective. We managed to do some in-lab testing as well, although not as much as the previous period. 

The only thing that is left is to merge the last task, test everything, and record a demo.

## Development
In sub chapters bellow are included updates that were completed in Final Handin milestone.

### Meteorite Explosion
Meteorite explosion was created to add extra feature to meteorites that were added in the previous week and also to simulate a real collision. To create an explosion effect was used particle system. It was necessary to update the settings of particle system and to play around with it to achieve a desired effect. `AsteroidManager` script was updated to trigger the explosion each type when either X-wing or Enemy rocket collides with meteorite. In the figure bellow can be see how was the script updated. If the collision occurs then the explosion is instantiated with the transform position of the object that the script is assigned to, so in this case the meteorite itself. The after 0.1 second is meteorite destroyed and after 2 seceonds also the explosion is destroyed from the scene.

<img width="600" alt="Screenshot 2024-11-24 at 18 51 08" src="https://github.com/user-attachments/assets/627ad92e-b339-416b-b59e-1191d963b341">

### Main menu Screen


### Pause menu Screen
