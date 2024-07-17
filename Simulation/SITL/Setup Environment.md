___
1. Clone the Gazebo simulation environment for [wingman](../../Wingman/Wingman.md)

```bash
cd /to/path/
git clone -b gazebo --single-branch https://gitlab.com/amadoruavs/wingman.git
```

2. Run `__main__.py` in a new terminal (`ctrl+alt+t`) after starting [SITL in PX4](Setup%20SITL.md) and [QGroundControl](../../GCS/QGroundControl.md)

```bash
cd /to/path/gazebo/
python3 .
```

3. Wait for `On the Ground` to be outputted in the terminal before starting the mission in QGroundControl

> [!NOTE]
> All terminal outputs will be viewable in QGroundControl by clicking on the megaphone on the top left corner of the screen.
___

### Sync with Jetson Wingman Changes

>[!WARNING]
>Before pushing any new code to the Jetson, ensure that it is tested in simulation and follows the mission and offboard control accurately.

In order for `__main__.py` to be executable in SITL without the need of a physical camera or Pixhawk, some minor edits must be made in order to sync changes.

#### General Instructions
1. Change `pixhawk_found`, `usb_drive_found`, and `camera_found` to True from False
2. Change the path of the `shape_model` to `YOLO("/home/amadoruavs/wingman/shapeclassifier2024_modified.pt")`
3. Change `await drone.connect()` to `await drone.connect('serial://' + pixhawk_path + ':921600')`
4. When reading the input using the camera and saving the image in local storage, change the code from the feed of the simulated camera to `image = cv2.cvtColor(cap.get_next_data(), cv2.COLOR_RGB2BGR)`