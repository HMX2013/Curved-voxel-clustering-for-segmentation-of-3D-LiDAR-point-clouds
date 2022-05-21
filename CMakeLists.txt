cmake_minimum_required(VERSION 3.0.2)
project(cvc_segment_origin)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  pcl_ros
  tf2_ros
  tf2_geometry_msgs
  dynamic_reconfigure
  autoware_msgs
  jsk_recognition_msgs
)

find_package(OpenCV REQUIRED)

set(CMAKE_CXX_FLAGS "-O2 -Wall ${CMAKE_CXX_FLAGS}")

generate_dynamic_reconfigure_options(
  cfg/cvc_segment_origin.cfg
)


catkin_package(
  INCLUDE_DIRS include
  LIBRARIES cvc_segment_origin
  CATKIN_DEPENDS roscpp std_msgs
  # DEPENDS system_lib
)


## Specify additional locations of header files
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${dynamic_reconfigure_PACKAGE_PATH}/cmake/cfgbuild.cmake
  ${OpenCV_INCLUDE_DIRS}
)

link_directories(${OpenCV_LIBRARY_DIRS})

## Declare a C++ library
# add_library(${PROJECT_NAME}
#   src/dbscan_kdtree.hpp
# )

## Add cmake target dependencies of the library
## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
add_executable(cvc_segment_origin 
  src/cvc_segment_origin.cpp
  src/cluster.cpp
)

## Add cmake target dependencies of the executable
## same as for the library above
add_dependencies(cvc_segment_origin 
  ${${PROJECT_NAME}_EXPORTED_TARGETS} 
  ${catkin_EXPORTED_TARGETS}
)


## Specify libraries to link a library or executable target against
target_link_libraries(cvc_segment_origin
  ${OpenCV_LIBRARIES}
  ${catkin_LIBRARIES}
)