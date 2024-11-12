
---
## **Linux**

---

> [!NOTE]
> **Note**  
> These instructions assume you already have Ubuntu 20.04+ booted and now want to setup SITL.  
> *If you have not already, Ubuntu 22.04 LTS (Jammy) is recommended.*

---

> [!IMPORTANT]
> **Recommended**  
> This is only available on Ubuntu 20.04 and 22.04.

### Gazebo Classic (Supported by 20.04 and 22.04)

1. Clone the PX4 Developer Toolchain
```bash
mkdir src
cd src
git clone --recursive https://github.com/PX4/PX4-Autopilot.git
```

2. Run `ubuntu.sh` to install everything
```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

3. Restart the computer on completion
```bash
sudo reboot
```

4. Run the simulation by starting PX4 SITL
```bash
cd PX4-Autopilot
make px4_sitl gazebo-classic
```

 - See all supported vehicles: [Supported Vehicles](https://docs.px4.io/main/en/sim_gazebo_classic/vehicles.html#quadrotor-default)
 
| Vehicle       | Command                                      | 
| ------------- | -------------------------------------------- | 
| Quadrotor     | `make px4_sitl gazebo-classic`               | 
| Standard VTOL | `make px4_sitl gazebo-classic_standard_vtol` | 

---

> [!WARNING]
> **Warning**  
> Only use if you already have Ubuntu 24.04.

### Gazebo (Supported by 24.04)

1. Clone PX4-Gazebo-Jammy *(thanks to Vardaan)*
```bash
git clone https://github.com/HairyOtter07/PX4-Gazebo-Jammy.git
```

2. Run `setup.sh`
```bash
cd PX4-Gazebo-Jammy
./setup.sh
```

3. Run the simulation by starting PX4 SITL
```bash
./run.sh
```

> [!NOTE]
> **Note**  
> The default vehicle will be the `gz_x500`
> Supported vehicles: [Supported Vehicles](https://docs.px4.io/main/en/sim_gazebo_gz/vehicles.html)

#### Changing the Default Vehicle
1. Open the `docker-compose.yml`
2. Edit the `command` by replacing
```bash
bash -c 'cd /src/PX4-Autopilot && make px4_sitl gz_x500'
```
with
```bash
bash -c 'cd /src/PX4-Autopilot && make px4_sitl gz_{VEHICLE}_{WORLD}'
```

---
## **MacOS**

---
> [!IMPORTANT]
> **Apple Silicon MacOS**  
> If you have an Apple M1, M2 etc. Macbook, make sure to run the terminal as x86 by setting up an x86 terminal:
> 1. Locate the Terminal application within the Utilities folder (**Finder > Go menu > Utilities**)
> 2. Select _Terminal.app_ and right-click on it, then choose **Duplicate**.
> 3. Rename the duplicated Terminal app, e.g. to _x86 Terminal_
> 4. Now select the renamed _x86 Terminal_ app and right-click and choose *_Get Info_
> 5. Check the box for **Open using Rosetta**, then close the window
> 6. Run the _x86 Terminal_ as usual, which will fully support the current PX4 toolchain

### Gazebo Classic

1. Set up MacOS environment

```bash
echo ulimit -S -n 2048 >> ~/.zshenv
# Point pip3 to MacOS system python 3 pip
alias pip3=/usr/bin/pip3
```

2. Install common tools

```bash
brew tap PX4/px4
brew install px4-dev
```

3. Install required Python packages

```bash
# install required packages using pip3
python3 -m pip install --user pyserial empty toml numpy pandas jinja2 pyyaml pyros-genmsg packaging kconfiglib future jsonschema
# if this fails with a permissions error, your Python install is in a system path - use this command instead:
sudo -H python3 -m pip install --user pyserial empty toml numpy pandas jinja2 pyyaml pyros-genmsg packaging kconfiglib future jsonschema
```

3. Setup Gazebo Classic
```bash
brew unlink tbb
sed -i.bak '/disable! date:/s/^/  /; /disable! date:/s/./#/3' $(brew --prefix)/Library/Taps/homebrew/homebrew-core/Formula/tbb@2020.rb
brew install tbb@2020
brew link tbb@2020
```

4. Install SITL simulation
```bash
brew install --cask temurin
brew install --cask xquartz
brew install px4-sim-gazebo
```

5. Clone the PX4 Developer Toolchain
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
cd PX4-Autopilot/Tools/setup
sh macos.sh
```

4. Run the simulation by starting PX4 SITL
```bash
cd PX4-Autopilot
make px4_sitl gazebo-classic
```

---

### Common Errors

1. Homebrew is looking for the `gettext` library in the wrong folder.  

**Solution**: Create a symbolic link to point Homebrew to the correct folder.
``` bash
ln -s "/Users/{errored user}/homebrew" "/Users/{correct user}/homebrew"
```

---
## **Testing SITL**

---

1. [Run QGroundControl](../../GCS/QGroundControl.md#Running)
>[!NOTE]
>If QGroundControl is not already installed on the computer, then follow the [Installation Instructions](../../GCS/QGroundControl.md).

2. Run the simulation by starting PX4 SITL

If everything was set up correctly, QGroundControl will connect automatically!

---

> [!IMPORTANT]
> **Try** creating a mission using QGroundControl and executing it. You should be able to see the drone in Gazebo arm, takeoff, and follow the mission as specified.

___

The drone will appear to be in a random location, such as 

| World       | (Latitude, Longitude)                                      | 
| ------------- | -------------------------------------------- | 
| Gazebo (`default.sdf`)     | `(47.398, 8.546)`           | 
| Gazebo Classic (`baylands.world`) | `(37.414, -121.996)` | 


In order to change the home coordinates of the drone, you must edit the `.sdf` (Gazebo format) or `.world` (Gazebo Classic format).

| Version       | Path                                      | 
| ------------- | -------------------------------------------- | 
| Gazebo         | `/path/to/PX4-Autopilot/Tools/simulation/gz/worlds/{WORLD}.sdf/`           | 
| Gazebo Classic | `/path/to/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds/{WORLD}.world/` | 

1. Open the file depending on what version you have installed
2. Find the `<spherical_coordinates>`  tag
3. Edit the `<latitude_deg>` and `<longitude_deg>` values accordingly.

| Location       | (Latitude, Longitude)                                      | 
| ------------- | -------------------------------------------- | 
| Pleasanton     | `(37.6772, -121.8866)`           | 
| Competition | `(38.3151, -76.54884)`|

---
