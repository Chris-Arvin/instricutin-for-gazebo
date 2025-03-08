# Quick instructions for building a gazebo env

### Step 1: go into docker

```bash
xhost +
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
```

### Step 2: DIY an environment with GUI

```bash
source /opt/ros/humble/setup.bash
gazebo
```

You have three model options: box, sphere, and cylinder. Click one to load it, move the mouse and drag it into the desired place, and click again to confirm the place.

![img](https://github.com/osrf/gazebo_tutorials/raw/master/build_world/files/empty_world_simple_shapes_highlighted.png)

After placing a model, you can change its shape with three operations: translation, rotation, and scale.

![img](https://github.com/osrf/gazebo_tutorials/raw/master/build_world/files/empty_rts.png)

**See https://classic.gazebosim.org/tutorials?tut=build_world for more instructions.**

### Step 3: save your environment

Click: **file**->**save world as**, to save the world. Make sure the "location" is:

```bash
/usr/app/comp0244_ws/comp0244-go2/src/unitree-go2-ros/robots/configs/go2_config/worlds
```

and the file name is ended with ".world" (like test1.world). 

Just like: 
![Screenshot from 2025-03-08 09-19-02](https://github.com/user-attachments/assets/6475a100-6747-4cab-9066-c63ddb9977c9)


### Step 4: assign models with static attribute

Open test1.world with gedit:

```bash
gedit /usr/app/comp0244_ws/comp0244-go2/src/unitree-go2-ros2/robots/configs/go2_config/worlds/test1.world 
```

Use **Find and Replace** to quickly assign static attributes to all obstacles:
![Screenshot from 2025-03-08 09-29-12](https://github.com/user-attachments/assets/cd427d8c-0bcf-4b31-b014-edd6b17459e2)


Find:

            <kinematic>0</kinematic>
          </link>
        </model>

Replace with:

            <kinematic>0</kinematic>
          </link>
          <static>1</static>
        </model>

Finally, replace all and save the file.

### Step 5: open your environment alongside unitree_go2
Open **gazebo_mid360.launch** and change the **default_world_path**
```bash
gedit /usr/app/comp0244_ws/comp0244-go2/src/unitree-go2-ros2/robots/configs/go2_config/launch/gazebo_mid360.launch.py
```
Change the original **default_world_path** from:
```bash
default_world_path = os.path.join(config_pkg_share, "worlds/simple_environment.world")
```
into:
```bash
default_world_path = os.path.join(config_pkg_share, "worlds/test1.world")
```
rebuild the env and start it:
```bash
cd /usr/app/comp0244_ws/comp0244-go2 && colcon build --packages-select go2_config
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash

```
