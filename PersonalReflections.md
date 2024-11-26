# Personal Reflections

## Simon
### General reflections
At the beginning of the semester I thought AR will be an easier than VR in terms of development and I was wrong. I am not sure if it was the selection of the topic for the project itself that made it harder but we struggled with it a lot (at least that was my feeling). Based on that, I wish there was more time dedicated to developing the VR project, as the school has dedicated headsets on which the testing is much better. 

From the lecture perspective, I understand that this course is build mainly on us resarching and implementing the projects, but I think it would be beneficial the teacher could do like an introductory demo of setting up project specific scenes in the class. We get access to some already made demos but the reverse engineering takes quite some time and it feels like you are just spending time on basic stuff that could be done in few minutes if you knew how. I for example, struggled with understanding how to create a custom script for interactable, as I needed a grabable object that would stay in place (a joystick) and there was not enough guidance/explanantion online (I just ended up getting pre-implemented script but had to spend additional time on fine tuning it).

### AR Project
First of all, testing on Samsung is basically impossible for our project, as it just doesn't scan the wall properly, you have to realy move it around and hope it will register something on the wall for registering the plane (even on walls with a lot of decoration). I am not sure how well it then works when you have a pre-modeled environment, but if it is better, maybe it would be a good idea to steer students who test on Samsung in that direction.

Regarding our project, there were some issues with the implementation as we only tested on 1 wall (either in editor or when deployed to phone). When we (maybe the last week of the project) 'accidentally' tested on other walls, the rotation of our objects placed on that wall was wrong. This was because we were taking use of the axis when placing the object but the axis of the project does not rotate together with the device (funny that we did not think about this earlier). I believe the way to fix this would be not placing the objects with the help of the project's dimensions but with the plane on which the object is placed.

### VR Project
This one was really fun (sometimes). We came up with a great idea. flight simulator with a StarWars setting. Initially we thought we will only have time to implement the flying mechanics and maybe add some space decoration like meteorites, but we ended with with a decent game where there are enemies, meteorites, imperial cruiser and you can shoot stuff. It really gives a great immersion and you feel like flying in the space (especially as the joystick for maneuvering the spaceship is grabable, as is the thrust controller, so it really feals like you are in charge of the spaceship).  

The only bigger issue was when it came to implementing the grabable joystick and thrust controller. We wanted to make it from scratch as the behaviour of those objects is not anything you can find in the default ones provided by the XR Interaction Toolkit samples. However, it was really challenging to find a good guide or documentation on how to create those specific implementations and it was hard to try stuff out as for that, you had to be in school to try it directly on the headset (most of us also has Mac so it is ever harder with that). We ended up taking some already pre-implemented scripts and managed to fine tune them so that they work as we wanted them to.

As mentioned above, it would be great to have some more time for developing this project as it was way more fun than the AR one (but this feeling could well be due to the topics we chose for the individual projects).

## David 

## Anton

## James
