# DevBlog 3

## Process
We experienced bit of a difficulty while trying to finish our MVP on time, some of the tasks were a bit more complex than we originaly expected (especially placing an object in the correct orientation or not being able to move placed objects into each other).

To make out for it, we refactored fome of the tasks and only implemented the parts that were really necessary for our MVP so that the team could move forward and start with implementing the core features. We also did a few pair programming / debuging sessions to speed up the progress.

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
