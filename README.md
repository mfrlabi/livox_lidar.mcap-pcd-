# livox_Mid360 .mcap & .pcd & icp


Livox ros_diver2 ( using Livox Mid360 lidar to run in the rviz and save data )
  -The published point cloud topic /livox/lidar is in the format livox_ros_driver/CustomMsg 
  and save point cloud data in .pcd format



1. 

*** to connect your pc with Livox Mid360 lidar via Ethernet:   ( Change the IP address according to your device )

IPv4 > manual 
             
			 address: 192.168.1.5
             gateway: 192.168.1.200
						 
	

    source /opt/ros/jazzy/setup.bash 
    source install/setup.bash 
			

source install/setup.bash   - in directory:   labi@lb:~/ws_livox$  source install/setup.bash 


           


2. to collect msg_custom messages   - in directory:      labi@lb:~/ws_livox$ 
          
     ros2 launch livox_ros_driver2 msg_MID360_launch.py 

After running (	ros2 launch livox_ros_driver2 msg_MID360_launch.py )  in the Terminal keep it unchanged 		



3. to visualize point cloud data  - in directory:    labi@lb:~/ws_livox$  
           
     ros2 launch livox_ros_driver2 rviz_MID360_launch.py 

Note: do not run 2 and 3 together  (only to visualize lidar data, never run when you run msg_MID360_launch.py to save pcd and other data)




4. to check ros topic list in labi@lb:~/ws_livox$   ( optional )
   
     ros2 topic list

   
				 
/livox/imu
/livox/lidar
/parameter_events
/rosout
labi@lb:~/ws_livox$ 



5. 2nd Terminal ---- to check whether /livox/lidar is publishing data or not - in directory:    labi@lb:~/ws_livox$  ( not mandatory )

 
     ros2 topic echo /livox/lidar 


6. 3rd Terminal ---- to record ros bag file in .mcap format  - in directory:   labi@lb:~/ws_livox$   (for ubuntu 24 )
         
  
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

Now copy the file:     (or to customize or modify  pcd saver) 


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


7. icp 


Rebuild your package: (if you haven't build your icp package yet)

    cd ~/ws_livox
    colcon build --symlink-install --packages-select icp_pcd

or

    colcon build --symlink-install --packages-select icp_pcd --cmake-clean-cache


Source the workspace:

    source install/setup.bash



Verify:

    ls ~/ws_livox/install/icp_pcd/lib/icp_pcd/



You should now see both:

icp_pcd
hybrid_icp_pcd_vis


Run the visualizer:    ros2 run icp_pcd hybrid_icp_pcd_vis ~/pcd_output ~/pcd_output/transforms.txt 0.1

    ros2 run icp_pcd icp_pcd

    ros2 run icp_pcd hybrid_icp_pcd_vis ~/pcd_output ~/pcd_output/transforms.txt 0.1






