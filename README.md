# RTK定位

## 编译

```bash
cd ~/colcon_ws/deeprobotics_x30_ws
colcon build --packages-select nmea_msgs nmea_navsat_driver wheeltec_gps_driver wheeltec_dual_rtk_driver --symlink-install
```

## 启动

```bash
source ~/colcon_ws/deeprobotics_x30_ws/install/setup.zsh
ros2 launch wheeltec_gps_driver wheeltec_dual_rtk_driver_nmea.launch.py

source ~/colcon_ws/deeprobotics_x30_ws/install/setup.zsh
ros2 topic echo /gps/fix
ros2 topic echo /gnss/gpgga
ros2 topic echo /heading
```

## 接口

`/gps/fix`示例：

```text
header:
  stamp:
    sec: 1787642766
    nanosec: 228840593
  frame_id: navsat_link
status:
  status: -1
  service: 1
latitude: .nan
longitude: .nan
altitude: .nan
position_covariance:
- 9.9980001e+19
- 0.0
- 0.0
- 0.0
- 9.9980001e+19
- 0.0
- 0.0
- 0.0
- 1.599680016e+21
position_covariance_type: 0
```

`/gnss/gpgga`示例：

```text
header:
  stamp:
    sec: 1787642789
    nanosec: 502768397
  frame_id: navsat_link
message_id: ''
utc_seconds: 1787642789.0
lat: .nan
lon: .nan
lat_dir: ''
lon_dir: ''
gps_qual: 0
num_sats: 0
hdop: 9999.0
alt: .nan
altitude_units: M
undulation: .nan
undulation_units: M
diff_age: 0
station_id: ''
```

## 排错

```bash
sudo usermod -aG dialout "$USER"

echo 'SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="55d4", ATTRS{serial}=="5B31018904", MODE:="0660", GROUP:="dialout", SYMLINK+="wheeltec_gnss"' | sudo tee /etc/udev/rules.d/99-wheeltec-g90.rules

sudo udevadm control --reload-rules

ls -l /dev/wheeltec_gnss
lrwxrwxrwx 1 root root 7  8月 25 13:30 /dev/wheeltec_gnss -> ttyACM0

python3 -m serial.tools.miniterm /dev/wheeltec_gnss 115200 --raw
```
