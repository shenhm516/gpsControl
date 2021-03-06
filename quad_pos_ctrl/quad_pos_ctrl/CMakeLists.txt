cmake_minimum_required(VERSION 2.8.3)
project(quad_pos_ctrl)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  rospy
  std_msgs
  mavros
	ctrl_msg
  nodelet
  rosbag
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)

find_package(Eigen3)
if(NOT EIGEN3_FOUND)
  # Fallback to cmake_modules
  find_package(cmake_modules REQUIRED)
  find_package(Eigen REQUIRED)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIRS})
  set(EIGEN3_LIBRARIES ${EIGEN_LIBRARIES})
endif()

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES quad_pos_ctrl
  CATKIN_DEPENDS geometry_msgs roscpp rospy std_msgs mavros ctrl_msg nodelet
#  mav_trajectory_generation mav_trajectory_generation_ros
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
#include_directories(
# include
#  ${catkin_INCLUDE_DIRS}
#)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  include
)

## Declare a C++ library
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/quad_pos_ctrl.cpp
# )
add_library(quad_pos_ctrl
  src/controller.cpp
)

add_library(ctrl_nodelet
  src/CtrlNodeletClass.cpp
)

target_link_libraries(ctrl_nodelet
        quad_pos_ctrl
        ${catkin_LIBRARIES})

if(catkin_EXPORTED_LIBRARIES)
  add_dependencies(ctrl_nodelet ${catkin_EXPORTED_LIBRARIES})
endif()
## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
# add_executable(${PROJECT_NAME}_node src/quad_pos_ctrl_node.cpp)
# main node
add_executable(quad_pos_ctrl_node src/quad_pos_ctrl_node.cpp)
# test node
# add_executable(test_pub_node test/test_pub_node.cpp)
# add_executable(test_takeoff_node test/test_takeoff_node.cpp)
# add_executable(test_takeoff_node2 test/test_takeoff_node2.cpp)
# add_executable(test_takeoff_node3 test/test_takeoff_node3.cpp)
# add_executable(test_takeoff_node4 test/test_takeoff_node4.cpp)
add_executable(testTaskMode test/testTaskMode.cpp)
#add_executable(test_sample_mocap test/test_sample_mocap.cpp)
#add_executable(linear_optimal_test test/linear_optimal_test.cpp)
#add_executable(nonlinear_optimal_test2 test/nonlinear_optimal_test2.cpp)
#add_executable(linear_optimal_formation_test test/linear_optimal_formation_test.cpp)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(${PROJECT_NAME}_node
#   ${catkin_LIBRARIES}
# )

target_link_libraries(quad_pos_ctrl_node
  quad_pos_ctrl
  ${catkin_LIBRARIES}
)

# target_link_libraries(test_pub_node
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(test_takeoff_node
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(test_takeoff_node2
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(test_takeoff_node3
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(test_takeoff_node4
#   ${catkin_LIBRARIES}
# )

target_link_libraries(testTaskMode
  ${catkin_LIBRARIES}
)

#target_link_libraries(test_sample_mocap
  #quad_pos_ctrl
  #${catkin_LIBRARIES}
#)

#target_link_libraries(linear_optimal_test
#  ${mav_trajectory_generation_LIBRARIES}
#  ${mav_trajectory_generation_ros_LIBRARIES}
#)

#target_link_libraries(nonlinear_optimal_test2
#  ${mav_trajectory_generation_LIBRARIES}
#  ${mav_trajectory_generation_ros_LIBRARIES}
#)

#target_link_libraries(linear_optimal_formation_test
#  ${mav_trajectory_generation_LIBRARIES}
#  ${mav_trajectory_generation_ros_LIBRARIES}
#)
#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_quad_pos_ctrl.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
