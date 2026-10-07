cmake_minimum_required(VERSION 3.20)
project(ira_laser_tools)

# Default to C99
set(CMAKE_C_STANDARD 99)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(laser_geometry REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED)

# bugfix for g++-Warning
#   "<command-line>:0:0: warning: missing whitespace after the macro name"
remove_definitions(-DDISABLE_LIBUSB-1.0)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations

add_executable(laserscan_multi_merger src/laserscan_multi_merger.cpp)
target_include_directories(laserscan_multi_merger PUBLIC ${EIGEN_INCLUDE_DIRS})
target_include_directories(laserscan_multi_merger PUBLIC ${PCL_INCLUDE_DIRS})
target_link_libraries(laserscan_multi_merger PUBLIC
  ${geometry_msgs_TARGETS}
  ${sensor_msgs_TARGETS}
  ${std_msgs_TARGETS}
  laser_geometry::laser_geometry
  pcl_ros::bag_to_pcd_lib
  pcl_ros::combined_pointcloud_to_pcd_lib
  pcl_ros::pcd_to_pointcloud_lib
  pcl_ros::pcl_ros_filter
  pcl_ros::pcl_ros_filters
  pcl_ros::pcl_ros_tf
  pcl_ros::pointcloud_to_pcd_lib
  rclcpp::rclcpp
  rclcpp_action::rclcpp_action
  rclcpp_lifecycle::rclcpp_lifecycle
  sensor_msgs::sensor_msgs_library
  tf2::tf2
  tf2_geometry_msgs::tf2_geometry_msgs
  tf2_ros::static_transform_broadcaster_node
  tf2_ros::tf2_ros
  ${PCL_LIBRARIES}
  ${EIGEN_INCLUDE_DIRS}
)

add_executable(laserscan_virtualizer src/laserscan_virtualizer.cpp)
target_include_directories(laserscan_virtualizer PUBLIC ${EIGEN_INCLUDE_DIRS})
target_include_directories(laserscan_virtualizer PUBLIC ${PCL_INCLUDE_DIRS})
target_link_libraries(laserscan_virtualizer PUBLIC
  ${geometry_msgs_TARGETS}
  ${sensor_msgs_TARGETS}
  ${std_msgs_TARGETS}
  laser_geometry::laser_geometry
  pcl_ros::bag_to_pcd_lib
  pcl_ros::combined_pointcloud_to_pcd_lib
  pcl_ros::pcd_to_pointcloud_lib
  pcl_ros::pcl_ros_filter
  pcl_ros::pcl_ros_filters
  pcl_ros::pcl_ros_tf
  pcl_ros::pointcloud_to_pcd_lib
  rclcpp::rclcpp
  rclcpp_action::rclcpp_action
  rclcpp_lifecycle::rclcpp_lifecycle
  sensor_msgs::sensor_msgs_library
  tf2::tf2
  tf2_geometry_msgs::tf2_geometry_msgs
  tf2_ros::static_transform_broadcaster_node
  tf2_ros::tf2_ros
  ${PCL_LIBRARIES}
  ${EIGEN_INCLUDE_DIRS}
)

#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(TARGETS laserscan_multi_merger laserscan_virtualizer
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

## Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY launch/
  DESTINATION share/${PROJECT_NAME}/launch
)

ament_export_dependencies(${dependencies})
ament_package()
