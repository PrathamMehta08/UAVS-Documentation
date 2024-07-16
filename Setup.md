# SITL

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
 
| Vehicle                                | Command                                                   |
|----------------------------------------|-----------------------------------------------------------|
| Quadrotor                              | `make px4_sitl gazebo-classic`                               |
| Standard VTOL            | `make px4_sitl gazebo-classic_standard_vtol`              |                  |

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
