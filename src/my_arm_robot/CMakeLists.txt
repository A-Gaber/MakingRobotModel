cmake_minimum_required(VERSION 2.8.3)
project(my_arm_robot)

find_package(catkin REQUIRED COMPONENTS 
  message_generation 
  gazebo_msgs 
  roscpp 
  rospy 
  nodelet 
  angles 
  std_srvs 
  geometry_msgs 
  sensor_msgs 
  nav_msgs 
  urdf 
  tf 
  tf2_ros 
  dynamic_reconfigure 
  rosgraph_msgs 
  trajectory_msgs 
  image_transport 
  rosconsole
  cv_bridge
  polled_camera
  diagnostic_updater
  camera_info_manager
  std_msgs
  joy
)

include (FindPkgConfig)
if (PKG_CONFIG_FOUND)
  pkg_check_modules(XML libxml-2.0)
  pkg_check_modules(OGRE OGRE)
  pkg_check_modules(OGRE-Terrain OGRE-Terrain)
  pkg_check_modules(OGRE-Paging OGRE-Paging)
else()
  message(FATAL_ERROR "pkg-config is required; please install it")
endif()

# Depend on system install of Gazebo and SDFormat
find_package(gazebo REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

execute_process(COMMAND
  pkg-config --variable=plugindir OGRE
  OUTPUT_VARIABLE OGRE_PLUGIN_PATH
  OUTPUT_STRIP_TRAILING_WHITESPACE
)

catkin_python_setup()

#generate_dynamic_reconfigure_options(
#  cfg/CameraSynchronizer.cfg
#  cfg/GazeboRosCamera.cfg
#  cfg/GazeboRosOpenniKinect.cfg
#  cfg/Hokuyo.cfg
#)

include_directories(
  ${Boost_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
  ${GAZEBO_INCLUDE_DIRS}
  ${OGRE_INCLUDE_DIRS}
  ${OGRE-Terrain_INCLUDE_DIRS}
  ${OGRE-Paging_INCLUDE_DIRS}
)

link_directories(
  ${GAZEBO_LIBRARY_DIRS}
  ${OGRE_LIBRARY_DIRS}
  ${OGRE-Terrain_LIBRARY_DIRS}
  ${OGRE-Paging_LIBRARY_DIRS}
  ${catkin_LIBRARY_DIRS}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

#if (NOT GAZEBO_VERSION VERSION_LESS 6.0)
#  catkin_package(  INCLUDE_DIRS include LIBRARIES gazebo_ros_elevator)
#endif()

#if (NOT GAZEBO_VERSION VERSION_LESS 7.3)
#  catkin_package(INCLUDE_DIRS include LIBRARIES gazebo_ros_harness)
#endif()

catkin_package(
  INCLUDE_DIRS 
  LIBRARIES 
#  vision_reconfigure 
#  gazebo_ros_utils 
#  gazebo_ros_camera_utils 
#  gazebo_ros_camera 
#  gazebo_ros_multicamera 
#  gazebo_ros_depth_camera 
#  gazebo_ros_openni_kinect 
#  gazebo_ros_gpu_laser 
#  gazebo_ros_laser 
#  gazebo_ros_block_laser 
#  gazebo_ros_p3d 
#  gazebo_ros_imu 
#  gazebo_ros_f3d 
#  gazebo_ros_ft_sensor
#  gazebo_ros_bumper 
#  gazebo_ros_template 
#  gazebo_ros_projector 
#  gazebo_ros_prosilica 
#  gazebo_ros_force 
#  gazebo_ros_joint_trajectory 
#  gazebo_ros_joint_state_publisher 
#  gazebo_ros_joint_pose_trajectory
#  gazebo_ros_diff_drive
#  gazebo_ros_tricycle_drive
#  gazebo_ros_skid_steer_drive
#  gazebo_ros_video
#  gazebo_ros_planar_move
#  gazebo_ros_vacuum_gripper
  
  CATKIN_DEPENDS 
  message_generation 
  gazebo_msgs 
  roscpp 
  rospy 
  nodelet 
  angles 
  std_srvs 
  geometry_msgs 
  sensor_msgs 
  nav_msgs 
  urdf 
  tf 
  tf2_ros
  dynamic_reconfigure 
  rosgraph_msgs 
  trajectory_msgs 
  image_transport 
  rosconsole
  camera_info_manager
  std_msgs
  DEPENDS 
    gazebo 
    SDF
  )
#add_dependencies(${PROJECT_NAME}_gencfg ${catkin_EXPORTED_TARGETS})

#add_library(gazebo_ros_force src/gazebo_ros_force.cpp)
#target_link_libraries(gazebo_ros_force ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})

#add_library(gazebo_ros_diff_drive src/gazebo_ros_diff_drive.cpp)
#target_link_libraries(gazebo_ros_diff_drive ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#target_link_libraries(gazebo_ros_diff_drive gazebo_ros_utils ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})

#add_library(gazebo_ros_tricycle_drive src/gazebo_ros_tricycle_drive.cpp)
#target_link_libraries(gazebo_ros_tricycle_drive ${GAZEBO_LIBRARIES} ${Boost_LIBRARIES} ${catkin_LIBRARIES})
#target_link_libraries(gazebo_ros_tricycle_drive gazebo_ros_utils ${GAZEBO_LIBRARIES} ${Boost_LIBRARIES} ${catkin_LIBRARIES})

#add_library(gazebo_ros_skid_steer_drive src/gazebo_ros_skid_steer_drive.cpp)
#target_link_libraries(gazebo_ros_skid_steer_drive ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})

##
## Add your new plugin here
##

add_library(my_arm src/my_arm.cpp)
target_link_libraries(my_arm ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})

add_executable(teleop_joy src/teleop_joy.cpp)
target_link_libraries(teleop_joy ${catkin_LIBRARIES})


