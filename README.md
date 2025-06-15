## Robot Package Template

This is a GitHub template. You can make your own copy by clicking the green "Use this template" button.

It is recommended that you keep the repo/package name the same, but if you do change it, ensure you do a "Find all" using your IDE (or the built-in GitHub IDE by hitting the `.` key) and rename all instances of `my_bot` to whatever your project's name is.

Note that each directory currently has at least one file in it to ensure that git tracks the files (and, consequently, that a fresh clone has direcctories present for CMake to find). These example files can be removed if required (and the directories can be removed if `CMakeLists.txt` is adjusted accordingly).

## Learnings in no particular order

### Get going

**Get it running in Gazebo:**

```bash
source /opt/ros/humble/setup.zsh
cd path/to/where/src/folder/is
colcon build --symlink-install
ros2 launch my_bot launch_sim.launch.py world:=./src/my_bot/worlds/obstacles.world
```

**Get it running in Rviz:**

```bash
rviz2
```

And then open the file `drive_bot.rviz` in `src/config`.

**Drive the thing around:**

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

### Camera

**View camera topic outside of Rviz:**

```bash
sudo apt install ros-humble-rqt-image-view
```

```bash
ros2 run rqt_image_view rqt_image_view
```

**Remap compressed topic to an uncompressed one:**

```bash
sudo apt install ros-humble-image-transport-plugins
```

```bash
ros2 run image_transport republish compressed raw --ros-args -r in/compressed:=/camera/image_raw/compressed -r out:=/camera/image_raw/uncompressed
```

### ROS2 Control

**List hardware interfaces and controllers:**

```bash
ros2 control list_hardware_interfaces
```

```bash
ros2 control list_controllers
```

**Register(?) controllers in controller manager:**

Notice that when using Humble, it should be `spawner`, whereas in Foxy it should be `spawner.py`.

```bash
ros2 run controller_manager spawner joint_broad
```

```bash
ros2 run controller_manager spawner diff_cont
```

Also notice that these spawns are already done in the launch file.

**Remap topics for teleop_twist_keyboard to work with these controllers:**

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/diff_cont/cmd_vel_unstamped
```
