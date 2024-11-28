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
Start scene and Main menu were created to avoid starting the game loop directly when the users enter the game and instead place the user to virtual space, where he can start the game whenever he want. To create the Start scene and Main menu was used a unity package called **Lets make a VR game** from **[Valem Tutorials](https://www.youtube.com/@ValemTutorials)**. The package included following scripts: `GameStartmenu`, `Fader Screen`, `Scene Transition Manager` and `UI Audio manager`. After importing the asset and doing some small modification was Main Menu working. 'Milky Way' Skybox was added to the scene to increase the immersive experience. In the figure bellow can be seen Start scene with Main menu.

<img width="500" alt="Screenshot 2024-11-24 at 19 35 06" src="https://github.com/user-attachments/assets/27f3ea6e-778f-4a7c-b734-9e8083b87e65">

### Pause menu
Pause menu was created to allow user to navigate to Start Scene from Main Scene during the game loop. For the Pause Menu was used UI Canvas with two buttons. To make the UI buttons react to Raycast it was necessary to add `TrackedDeviceGraphicRaycaster` component to Canvas. 

In the figure bellow can be seen two methods from `PauseMenu` script, that were created to control Pause Menu UI. `OnEnable()` method is setting subscriber if the key that is responsible for toggling menu (Primary key on left controller) is pressed. If it's pressed then in `ToggleMenu()` method is the pause menu visibility toggled to opposite value and same applies for farcasting. The reason for enabling and disabling farcasting based on the PauseMenu visibilty is that farcasting is by default not used in the game, but we want to have it enabled only when interacting with UI.

<img width="500" alt="Screenshot 2024-11-24 at 19 45 51" src="https://github.com/user-attachments/assets/68cadae9-c501-48bf-b01a-751ff7dbe978">

In the figure bellow can be seen `Continue()` and `NavigateToMainMenu()` methods that are executed when Raycast hits Continue or Menu buttons. `Continue()` method is simply disabling the FarCasting and hiding the UI. `NavigateToMainMenu()` is navigating to Start Scene. 

<img width="318" alt="Screenshot 2024-11-24 at 19 52 00" src="https://github.com/user-attachments/assets/c154dfb3-6e3b-48fc-aa16-d9c9cc01d34b">

In the figure bellow can be seen how the Pause Menu looks like. For buttons and UI panel design was used **[Sci-fi GUI Asset](https://assetstore.unity.com/packages/2d/gui/sci-fi-gui-skin-15606)**
 from Unity Assets store.

<img width="500" alt="Screenshot 2024-11-24 at 19 40 19" src="https://github.com/user-attachments/assets/e23fa355-08ea-4e77-b2eb-cc036fca07b9">

## Cockpit screens
Every cockpit needs screens with info displaying. We added additional camera to `X-wing POV` prefab to have overview of what is hapening behind the x-wing. `CameraRenderTexture` was created and assigned to *Output Texture* of `RearViewCamera` object. Then this camera could be displayed on the screen, which is basically `Quad` with `CameraRenderTexture` as material. See the image bellow for final result. 

For speed displaying, another controller was created called `SpeedDisplayController.cs`. It has reference to `VRFlightController` to access *currentSpeed*, *minSpeed* and *topSpeed*. Function bellow controlls *displayedSpeed* as number and *speedBar*.
```
	void Update()
	{
		if (flightController != null && speedDisplay != null)
		{
			currentSpeed = flightController.GetCurrentSpeed();

			// Calculate displayed speed (mapped to 110 max)
			float displayedSpeed = (currentSpeed - minSpeed) * (maxDisplaySpeed - minSpeed) / (topSpeed - minSpeed) + minSpeed;
			speedDisplay.text = $"{displayedSpeed:F1}";

			if (speedBar != null)
			{
				float fillAmount = Mathf.InverseLerp(minSpeed, topSpeed, currentSpeed);
				speedBar.fillAmount = fillAmount;
			}
		}
	}
```

<img width="525" alt="Screenshot 2024-11-25 at 12 37 49 pm" src="https://github.com/user-attachments/assets/e2e978d1-a8b8-4d96-956f-e5f37b5818b4">

There is still a lot of things that could be added to cockpit. We could add real blinking lights instead of textures, make more buttons or sticks interactable (for example Battle mode button) or shield health bar. We enjoyed working on this project very much and would have welcomed additional time to implement more features. However, due to time constraints, these additional features had to be left for future development.

### Enemy spawn and health
The enemies need to be able to be spawned, which is an excellent option to utilise object pooling.
There are two elements that need to be implemented:
- Enemy Spawner
- Enemy Pool

The EnemySpawner handles the spawning of the enemy objects in the environment. It uses a co-routine to begin spawining and running through the pool of enemies with a delay, giving time for enemies to be separated. Once it runs out enemies in the pool, it will stop the co-routine. Introducing the ResumeSpawning() method restarts the spawn from the pool. By setting the update method to check if the number of enemies in the pool is greater than 5, and calling the ResumeSpawning() method when true, it allows for an endless loop of enemies!

For this to work, the Enemy Pool also needs to exist. It is a simple script that enqueues a given number of enemies into queue object and exposes a GetEnemy() method and ReturnEnemy() method to dequeue and enqueue enemy objects respectively.

````
    public GameObject GetEnemy()
    {
        if (enemyPool.Count > 0)
        {
            GameObject enemy = enemyPool.Dequeue();
            EnemyHealth enemyHealth = enemy.GetComponent<EnemyHealth>();
            if (enemyHealth != null)
            {
                enemyHealth.OnEnemyDeath += ReturnEnemy;
            }
            enemy.SetActive(true);
            return enemy;
        }
        else
        {
            return null;
        }
    }

    public void ReturnEnemy(GameObject enemy)
    {
        EnemyHealth enemyHealth = enemy.GetComponent<EnemyHealth>();
        enemyHealth.Reset();
        enemyHealth.OnEnemyDeath -= ReturnEnemy;
        enemy.SetActive(false);
        enemyPool.Enqueue(enemy);
    }
````

 In the above methods, it is clear an EnemyHealth script is needed, which will be covered below. However, it is important to highlight the subscription and desubscription of the Enemy objects to an Action called "OnEnemyDeath" – where the action is invoked at some point in EnemyHealth and the ReturnEnemy method is called.

````
    public void Die()
    {
        Debug.Log("Boom");
        Instantiate(explosionPrefab, transform.position, transform.rotation);
        OnEnemyDeath?.Invoke(gameObject);
    }
````
Inside the EnemyHealth script, the Die() method is the logical place for this to be invoked, making a responsive return of the enemies into the waiting enemy pool. This handles all death whether it is from the player, from a collision, or some future implementation. 
