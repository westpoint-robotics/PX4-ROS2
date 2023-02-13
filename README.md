# PX4-ROS2
Repo to explain how to implement ROS2-PX4 in real hardware (companion computer to Pixhawk 4)

1. Pixahwk Firmware - 1.14beta version
- Install the firmware in Pixhawk 4 through QGC
- Important**
  Unlike previous companion computer settings like changing parameters (MAV_X_CONFIG, etc), PX4 team finally made it simpler for the users
  You do not have to enable any serial ports for MAVLINK, just change following parameters
  -XRCE_DDS_0_CFG = (TELEM1 or TELEM2)
  -SER_TEL1_BAUD or SER_TEL0_BAUD to the desire baudrate (921600N1)
- Connect Pixhawk to your companion computer through serial port (either TELEM1 or TELEM2) using USB-to-TTL cable
- The settings above will run microdds_client when the pixhawk is booted, thus all you have to do is to run MicroXRCEAgent on your companion computer

2. In your ROS2 workspace
-mkdir src
-git clone https://github.com/PX4/px4_msgs.git
-git clone https://github.com/PX4/px4_ros_com.git src/px4_ros_com

Then build them
-colcon build

3. XRCE-DDS: PX4-Fast DDS Bridge installation
- Link: https://github.com/PX4/PX4-user_guide/blob/d88460a7594d3930f621d645b461750a29d61cad/en/middleware/xrce_dds.md#xrce-dds-client-setup-for-flight-controllers
- Recommend to follow "Install Standalone From Source"

4. Operation
- Boot up your Pixhawk
- Make sure the Pixhawk is connected to your companion computer with USB-to-TTL cable from TELEM1 or TELEM2 port
- Open a terminal 
- Command: MicroXRCEAgent serial --dev /dev/ttyUSB0 -b 921600

5. Sanity Check
- Open another terminal to check if desired PX4 topics are published
- Command: ros2 topic list
- Command: ros2 topic echo /fmu/out/vehicle_odometry
- If you see the data is being streamed, you are good to go
