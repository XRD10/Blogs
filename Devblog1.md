The analysis phase
-
Following group formation, we presented several ideas for an AR Mobile application and discussed the potential uses and scale of the projects.

From this, we decided to develop an application that would provide the ability to arrange and place virtual representations of artwork and photos onto a real wall, with the goal of helping to visualise the planning of decorations
before commiting to mounting them. 

Once we decided on the project, we began to analyse the problem. We broke the project into user stories without limiting ourselves by time or complexity, but fully embracing the potential of the project. When we felt like we had presented
a feature-rich application, we took the user stories and grouped them into requirements that we could rank by importance and turn into tasks.

The first stage was to decide on the minimal viable product, where the application could fulfil the most basic solution to the problem: Visualise planning of decoration. We continued to separate the requirements into core and extension parts
to help plan the timetable for our work load. The tasks are as follows:

MVP
- Scan a wall and create a vartical plane within the wall
- Being ale to choose predefined ratio of a frame (both portrait and landscape)
- Being able to place object
- Being able to move object around
- Being able to scale an object within the app
- I want to create more than one object withnin one scene
- Being able to save a project

Core
-Use markers as reference points (measurement points) - should be presented to user as better app experience
- Use real time occlusion
- I want to know the real size of the objects (e.g. 10x20 cm)
- Upload a compressed version of a photo
- Attaching a compressed photo to the premodelled frame object
- I want to have a predefined mounting point on each frame
- I want to have an instruction of how to define my working zone (e.g. put a black cross tape in each corner)
- I want to locate the mounting points relative to the working zone (e.g. x,y coordinates (10cm from top and 20 cm from right)

Extension
- I want to have different style of photo frames
- Being able to project a project on a wall with a projector (extension of the base)

Following the dividison of tasks, the project repository was created in GitHub, and the initial commit from Unity was planned. This lead to an important discussion about how we would manage the workflow between GitHub and Unity. We decided
this would need further planning and testing to be sure it works as intended.

Finally, we decided on our timeframe for which we planned to keep ourselves 

What we did:
- Created requirements
- Divided into tasks
- Initialised project (Github)
- Discussed architecture

- Discussed workflow (ways of working)
    - **Main** for each screen in Unity – This will always be 'release' or workable implementation
    - **Branch** or **Fork** the main to develop features / per task
    - Create our own scenes? We will need to visualise this.
    - Get approved pull request --> Pull origin main --> Merge Local Dev scene into Local Main scene --> Merge Local Dev into Main branch
 
- Started to work on requirements 1 - 7
