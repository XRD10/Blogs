# X-Wing VR - Part 3

## Process
The final part of the VR project was really strong for our team as we were motivated from how the project has been developing we stayed very productive throughout the whole period and managed to finish most of the tasks, as can be seen in the figure below (there is one open task left and at the time of writing this post it is mostly implemented and not far from being reviewed, so it is on track):

![image](https://github.com/user-attachments/assets/f7585943-6279-4486-b94c-1de17cdc373a)

In this final period of the project, the team worked mostly online, but with a strong communication we managed to stay synchronized and effective. We managed to do some in-lab testing as well, although not as much as the previous period. 

The only thing that is left is to merge the last task, test everything, and record a demo.

## Development
In sub chapters bellow are included updates that were completed in Final Handin milestone.

### Meteorite Explosion
Meteorite explosion was created to add extra feature to meteorites that were added in the previous week and also to simulate a real collision. To create an explosion effect was used particle system. It was necessary to update the settings of particle system and to play around with it to achieve a desired effect. `AsteroidManager` script was updated to trigger the explosion each type when either X-wing or Enemy rocket collides with meteorite. 

In the figure bellow can be see how was the script updated. If the collision occurs then the explosion is instantiated with the transform position of the object that the script is assigned to, so in this case the meteorite itself. After 0.1 second is meteorite destroyed and after 2 seconds is explosion also destroyed from the scene.

<img width="600" alt="Screenshot 2024-11-24 at 18 51 08" src="https://github.com/user-attachments/assets/627ad92e-b339-416b-b59e-1191d963b341">

### Start scene 
Main menu were created to avoid starting the game loop directly when the users enter the game and instead position the user to virtual space, where he can start the game whenever he want. To create the Start scene and Main menu was used from a unity package called **Lets make a VR game** from **[Valem Tutorials](https://www.youtube.com/@ValemTutorials)**. The package included following scripts: `GameStartmenu`, `Fader Screen`, `Scene Transition Manager` and `UI Audio manager`. After importing the asset and doing some small modification was Main Menu working. To make the scene nicer was also added the 'Milky Way' Skybox. In the figure bellow can be seen Start scene with Main menu.

<img width="500" alt="Screenshot 2024-11-24 at 19 35 06" src="https://github.com/user-attachments/assets/27f3ea6e-778f-4a7c-b734-9e8083b87e65">

### Pause menu
Pause menu was created to allow user to navigate to Start Scene from Main Scene during the game loop. For the Pause Menu was used UI Canvas with two buttons. To make the UI buttons react to Raycast it was necessary to add `TrackedDeviceGraphicRaycaster` component to Canvas. In the figure bellow can be seen two methods from `PauseMenu` script, that was created to control Pause Menu UI. `OnEnable` method is setting subscriber if the key that is responsible for toggling menu(Primary key on left controller) is pressed. If it's pressed then the pause menu visibility is toggled to opposite value and same farcasting. The reason for enabling and disabling farCasting based on the PauseMenu visibilty is that farcasting is by default not used in the game, but we want to have it enabled only when interacting with UI.

<img width="500" alt="Screenshot 2024-11-24 at 19 45 51" src="https://github.com/user-attachments/assets/68cadae9-c501-48bf-b01a-751ff7dbe978">

In the figure bellow can be seen `Continue()` and `NavigateToMainMenu()` methods that are executed when Raycast hits Continue or Menu buttons. `Continue()` method simply disabled the FarCasting and hides the UI. `NavigateToMainMenu()` navigates to Start Scene. 

<img width="318" alt="Screenshot 2024-11-24 at 19 52 00" src="https://github.com/user-attachments/assets/c154dfb3-6e3b-48fc-aa16-d9c9cc01d34b">

In the figure bellow can be seen how the Pause Menu looks like. For buttons and UI panel design was used **[Sci-fi GUI Asset](https://assetstore.unity.com/packages/2d/gui/sci-fi-gui-skin-15606)**
 from Unity Assets store.

<img width="500" alt="Screenshot 2024-11-24 at 19 40 19" src="https://github.com/user-attachments/assets/e23fa355-08ea-4e77-b2eb-cc036fca07b9">
