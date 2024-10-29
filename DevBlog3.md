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

Bellow is shown `applyPicture()` method that is responsible for placing the picture that was chosen from gallery into the frame on the AR plane. In the method is created sprite from the texture that is essentially the picture chosen from the gallery. The reason for using Sprite for placing the picture into the frame instead of using for example Raw Image is that the frame is a 3D object and the RawImage component can be used only for 2D objects.

Pictures that can be uploaded to the frame can have all possible sizes and ratios. Moreover, those pictures can be placed in a huge range of frames. Because of that, the `applyPicture()` must handle all of those scenarios. For that are used sprite renderer bounds of frame object and newly created Sprite from gallery picture. Those two values are divided and the result is the scale factor. After that, transform values x and y are multiplied by that scale factor. The result is that the image is either scaled down or up, such that it fits into the frame. 

In the method, there are conditions to check if the frame is either landscape or portrait. The reason for checking the orientation of the frame is that scaleFactor is calculated by using the sprite bounds of another axis in each of those scenarios. So, for example in the case of the landscape, we are using `bounds.size.y`, and in the case of the portrait  `bounds.size.x`. Moreover, the method checks if the picture itself is either landscape or portrait. Depending on that is scale factor multiplied by different values. So, in the case of landscape, we want to scale slightly less than in the case of portrait. 

<img width="898" alt="Screenshot 2024-10-29 at 10 02 49" src="https://github.com/user-attachments/assets/4cb1b33a-ea3e-4e29-b174-53557b1c7978">


Below is a screenshot from testing the app on the Android device. There are pictures with landscape and as well portrait orientations placed on frames with different orientations and sizes.

<img width="300" alt="Screenshot 2024-10-13 at 20 42 39" src="https://github.com/user-attachments/assets/7871cc90-049f-4f88-84d0-cdd3e1371f94">


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
Our next goal was to display the distances from the frame to the edges of the working zone. This proved to be quite challenging and required a considerable amount of time and effort. This is what we ended with.

First, when placing a frame, we add it to the DistanceManager in `FramePlacer.cs` with the line `distanceManager.AddFrame(instance);`. This allows us to calculate the necessary positioning and display it to the user, indicating where to drill for the mounting point.

In `TakeScreenshotAndSave()`, we toggle the distance display at both the beginning and end by adding `distanceManager.ToggleDistanceDisplay();`.

The calculation process is as follows:
```
Bounds workingAreaBounds = workingAreaPlane.GetComponent<Renderer>().bounds;
Vector3 mountingPosition = mountingPoint.position;

Vector3 workingAreaCenter = workingAreaPlane.transform.position;
float positionLeft = workingAreaCenter.y - workingAreaBounds.size.y / 2;
float positionRight = workingAreaCenter.y + workingAreaBounds.size.y / 2;
float positionTop = workingAreaCenter.z + workingAreaBounds.size.z / 2;
float positionBottom = workingAreaCenter.z - workingAreaBounds.size.z / 2;


float distanceLeft = Mathf.Abs(mountingPosition.y - positionLeft);
float distanceRight = Mathf.Abs(mountingPosition.y - positionRight);
float distanceTop = Mathf.Abs(mountingPosition.z - positionTop);
float distanceBottom = Mathf.Abs(mountingPosition.z - positionBottom);
```

However, because of the rotation problem, we weren't able to make this work as intended, and so we did't merged the branch to main.
<img width="327" alt="Screenshot 2024-10-27 at 8 41 38 pm" src="https://github.com/user-attachments/assets/d5c7ee6a-35c5-4768-96b1-b46bb3a69c03">


### AR Session recording and replay
_This feature is only accessible in the `43-try-to-record-an-environment-and-test-it-in-the-simulator` branch as it is not complete and therefore it did not make sense for us to merge it to the main branch._

At the end of the project, we wanted to experiment with the AR session recording/replay just to see if it would be any useful to us. Because of lack of time, we were only able to end up with being able to record the scene environment and camera features (position, rotation) but we are confident that it would be possible to record the plane and objects placed in a similar way as we recorded the camera's features:

#### Recording
We created a custom script with a class called `ARFrameData` with the `System.Serializable` attribute so that it was possible to convert the data into a JSON format that we stored the information in (we also created a class `ARSessionRecording` with `List<ARFrameData>` that stored all the frames):
```
[System.Serializable]
public class ARFrameData
{
    public float timestamp;
    public Vector3 position;
    public Quaternion rotation;
}
```
In the `Start()` method we subscribed to the `frameReceived` event of the `ARCameraManager` object, that would call the `OnFrameReceived` method to save the frames:
```
    void Start()
    {
        arCameraManager.frameReceived += OnFrameReceived;
    }

    void OnFrameReceived(ARCameraFrameEventArgs args)
    {
        var frameData = new ARFrameData
        {
            timestamp = Time.time,
            position = arCameraManager.transform.position,
            rotation = arCameraManager.transform.rotation
        };

        recordedFrames.Add(frameData);
    }
```

The recording would then be saved as one JSON file to the following location: `Path.Combine(Application.persistentDataPath, "ARSessionRecording.json")`

#### Playback
The most important thing in the playback was to create a new playback designated camera and disable the original AR camera, otherwise the main camera would override the playbakc data and we would not be able to see the playback. 

Then it was just a matter of loading the JSON file, extract frame after frame, check for its timestamp and load the camera's data (If we reached and the recording file, the recording would simply stop, this behaviour could be changed to go back to the beginning by setting the `currentFrameIndex = 0` if we wanted to test longer):
```
    void Update()
    {
        if (isPlaying && currentFrameIndex < playbackRecording.frames.Count)
        {
            var frame = playbackRecording.frames[currentFrameIndex];

            if (Time.time - startTime >= frame.timestamp)
            {
                arPlaybackCamera.transform.position = frame.position;
                arPlaybackCamera.transform.rotation = frame.rotation;
                currentFrameIndex++;
            }

            if (currentFrameIndex >= playbackRecording.frames.Count)
            {
                isPlaying = false;
            }
        }
    }
```

