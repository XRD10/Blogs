# DevBlog 3

## Process
We experienced bit of a difficulty while trying to finish our MVP on time, some of the tasks were a bit more complex than we originaly expected (especially placing an object in the correct orientation or not being able to move placed objects into each other).

To make out for it, we refactored some of the tasks and only implemented the parts that were really necessary for our MVP so that the team could move forward and start with implementing the core features. We also did a few pair programming / debuging sessions to speed up the progress.

About week into implementing the core features of our app, we realized we do not have enough time for implementing all the requirements we set out to complete, as some of them required more time than we anticipated. To be sure that we have a working concept at the end of the development cycle (29.10.2024) we had a meeting where we discussed what is necessary to have implemented for the app's usecase to be valid. Based on that discussion we again, refactored some of the tasks and decided to drop some that were less important.

This continuous development and refactoring that we showcased helped us to end up with a relevant working product at the end that we can be happy about and is definitely something we will apply to our future projects as well.

## Development
### Creating more than 1 object
The general idea here is that it should be possible to place multiple frames on a wall. We wanted to avoid 2 situations here:
1. Being able to place objects over each other
2. Being able to move object into each other

The first situation was straight forward, we just check whether the `RaycastHit` would hit an object with a tag `Placable` which is a tag we assigned the frames that we will be placing:

```
if (Physics.Raycast(ray, out RaycastHit hitObject))
  if (hitObject.transform.CompareTag(Tag.Placable.ToString())) return;
```
The second situation proved to be way more complex, we first tried to implement it using a method called `OverlapBox` (since our frames are rectangles) but after some time of not being successful we refactored the second situation into a new separate task as this is not a 'must to have' implementation for the MVP.

### Custom frame sizes
It should be possible for the user to select a custom sized frame. To achieve this we created a canvas that will appear when the user will click to place an object onto the AR plane: 

![image](https://github.com/user-attachments/assets/d1537455-56d9-42a9-a4a8-493e0fdf1960)

 After pressing the **Set** button, the inputs will be read and assigned to the frame's `scale` within the `Transform` component. This functionality is assigned to the `onClick` event listener of the **Set** button withinthe `SetFrameSizesManager` as can be see non the figure below on lines **20-32**:

 <img width="887" alt="image" src="https://github.com/user-attachments/assets/a9771337-1f8b-42bc-84fc-b193b72fa265">

An interesting learning from implementing this task was that we expected that we will be setting X and Y axis with the custom values, however we learned that we actually needed to set X and Z. This, we believe, is because unity adjusts the coordinate system for vertical planes so that they keep consistency with the horizontal planes, therefore to make the Y axis to be the one represinting up/down from the plane it now faces towards the camera and X and Z are the ones describing the plane.

### Uploading a compressed version of a photo and attaching it to premodeled frame 
Another feature that was added to the current project is picking a picture that should be placed on the wall from the phone gallery. For that purpose was used asset [Native Gallery for Android & iOS](https://assetstore.unity.com/packages/tools/integration/native-gallery-for-android-ios-112630) that provides access to the phone gallery. 

Bellow is shown `openGallery()` method that is responsible for opening the gallery of the device and loading the picture. After the picture is loaded, then it is assigned to Texture2D variable from which is created sprite. Sprite is then assigned to spriteRenderer of the GameObject. SpriteRenderer is used because the GameObject to which should be the picture attached is 3D GameObject and other approaches such as using RowImage are not possible for 3D objects. This feature is still under development and it might be possible that the team will find out that assigning the picture to Sprite might cause some issues with quality of the picture and therefore will eventually use other approach.

<img width="995" alt="Screenshot 2024-10-13 at 20 49 54" src="https://github.com/user-attachments/assets/126bfd05-e981-48a4-8857-9b3612df3cc1">


Bellow is shown the picture that was uploaded from the gallery and was placed to the wall. There is currently issue that the picture is not addapting to the size of the current frame. That is something that will be fixed as the team will progress on developing this feature.

<img width="1046" alt="Screenshot 2024-10-13 at 20 42 39" src="https://github.com/user-attachments/assets/16181d16-029b-4b08-9779-657043cff657">


### Working area and distances of frame
Requirement no. 15: I want to locate the mounting points relative to the working zone (e.g. x,y coordinates (10cm from top and 20 cm from right)). 
The first step was to create a working zone, which was relatively straightforward. Initially, there were a few issues with the plane size, as it defaults to 10x10 meters. After some research, we used a Quad instead, which better suited our needs. Another challenge arose with correctly rotating the Quad to align with the detected plane, but we were able to resolve this as well.
```
// reference: https://discussions.unity.com/t/arfoundation-vertical-plane-recognition-position-rotation-on-plane-normal/786665
		Vector3 normal = -hitpose.up;
		Quaternion planeRotation = Quaternion.LookRotation(normal, Vector3.up);

		Vector3 adjustedPosition = hitpose.position + hitpose.up * 0.01f;
    workingAreaPlane = Instantiate(workingAreaPlanePrefab, adjustedPosition, planeRotation);
```
Our next goal was to display the distances from the frame to the edges of the working zone. This proved to be quite challenging and required a considerable amount of time and effort.


