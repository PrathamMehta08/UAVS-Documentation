### Installation

Before installing _QGroundControl_ for the first time:

1. On the command prompt enter:
    
    ```bash
    sudo usermod -a -G dialout $USER
    sudo apt-get remove modemmanager -y
    sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
    sudo apt install libfuse2 -y
    sudo apt install libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor0 -y
    ```
    
2. Logout and login again to enable the change to user permissions.

  To install _QGroundControl_:

1. Download [QGroundControl.AppImage](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage).
2. Install (and run) using the terminal commands:
    
    ```bash
    chmod +x ./QGroundControl.AppImage
    ./QGroundControl.AppImage  (or double click)
    ```

### Running

```bash
cd /to/path/QGroundControl
./QGroundControl.AppImage
```