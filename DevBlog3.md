# DevBlog 3

## Process
We experienced bit of a difficulty while trying to finish our MVP on time, some of the tasks were a bit more complex than we originaly expected (especially placing an object in the correct orientation or not being able to move placed objects into each other).

To make out for it, we refactored fome of the tasks and only implemented the parts that were really necessary for our MVP so that the team could move forward and start with implementing the core features. We also did a few pair programming / debuging sessions to speed up the progress.

## Implementation
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
