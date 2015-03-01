cmake_minimum_required(VERSION 2.8.3)
project(terabot_kinematics_plugin)


find_package(Eigen REQUIRED)
find_package(Boost REQUIRED thread)

find_package(catkin COMPONENTS
  moveit_core
  pluginlib
  actionlib
  roscpp
  rosconsole
  urdf
  kdl_parser
  tf
  tf_conversions
  eigen_conversions
  control_msgs
  REQUIRED)

catkin_package(
  LIBRARIES
  INCLUDE_DIRS
  DEPENDS
  moveit_core
  )

include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS} ${Boost_INCLUDE_DIR})
include_directories(include ${catkin_INCLUDE_DIRS})

link_directories(${Boost_LIBRARY_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

set(MOVEIT_LIB_NAME moveit_terabot_kinematics_plugin)

add_library(${MOVEIT_LIB_NAME} src/terabot_kinematics_plugin.cpp 
  src/chainiksolver_pos_nr_jl_mimic.cpp 
  src/chainiksolver_vel_pinv_mimic.cpp)

target_link_libraries(${MOVEIT_LIB_NAME} moveit_rdf_loader ${catkin_LIBRARIES})

install(TARGETS ${MOVEIT_LIB_NAME} LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
install(DIRECTORY include/ DESTINATION include)
