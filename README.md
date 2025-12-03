# livox_lidar.mcap-pcd-



1. Livox ros_diver2 ( using Livox Mid360 lidar to run in rviz and save data in)
  -The published point cloud topic /livox/lidar is in the format livox_ros_driver/CustomMsg 
  and save point cloud data in .pcd format


*** connect your pc with Livox Mid360 lidar via ethernet:

IPv4 > manual 
             
						 address: 192.168.1.5
             gateway: 192.168.1.200
						 
	

            source /opt/ros/jazzy/setup.bash 

labi@lb:~/ws_livox$  source install/setup.bash 



2. to collect msg_custom messages in labi@lb:~/ws_livox$ 
          
						ros2 launch livox_ros_driver2 msg_MID360_launch.py 

After run (	ros2 launch livox_ros_driver2 msg_MID360_launch.py )  in the Terminal keep it unchange		



3. to visualize point cloud data in labi@lb:~/ws_livox$  
           
					 ros2 launch livox_ros_driver2 rviz_MID360_launch.py 

note: do not run 2 and 3 together  (only to visualize lidar data, never run when you run msg_MID360_launch.py to save pcd and other data)




4. check ros topic list   ( optional )
         labi@lb:~/ws_livox$ ros2 topic list   
				 
/livox/imu
/livox/lidar
/parameter_events
/rosout
labi@lb:~/ws_livox$ 



5. 2nd Terminal ---- to check whether /livox/lidar is publishing data or not (it not mendatory)

   labi@lb:~/ws_livox$ 
         ros2 topic echo /livox/lidar 



7. 3rd Terminal ---- to record ros bag file in .mcap format (for ubuntu 24 )
         
   labi@lb:~/ws_livox$ 
       ros2 bag record /livox/lidar

				 

Record for exactly 30 seconds:
      timeout 30s ros2 bag record /livox/lidar
For multiple topics:
      timeout 30s ros2 bag record /livox/lidar /livox/imu

To avoid the warning "terminating with signal":
     timeout --signal=SIGINT 30s ros2 bag record /livox/lidar



6. Run the PCD saver node to save point cloud data in .pcd format

      ros2 run pcd_saver pcd_saver_custommsg


		
***-----------------------------------------------------------------------***
- Create the directory for the script
          mkdir -p ~/ws_livox/src/pcd_saver/pcd_saver

Now copy the file:  (or to customize or modify  pcd saver) 
          nano ~/ws_livox/src/pcd_saver/pcd_saver/pcd_saver_custommsg.py
					
Save (Ctrl+S), exit (Ctrl+X)
 make sure: Modify setup.py so ROS2 can run your script

         nano ~/ws_livox/src/pcd_saver/setup.py

Replace entry_points with this:
(
.
.
.
.

   entry_points={
       'console_scripts': [
           'pcd_saver_custommsg = pcd_saver.pcd_saver_custommsg:main',
           ],
       },
)

***Re-Build your workspace

cd ~/ws_livox
colcon build
source install/setup.bash

***Run the PCD saver node

        ros2 run pcd_saver pcd_saver_custommsg

***--------------------------------------------------------------------------------------------------***


