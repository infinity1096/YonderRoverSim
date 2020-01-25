cmake_minimum_required(VERSION 2.8.3)

project(rover)
set( CMAKE_CXX_FLAGS "-std=c++11" )
set(OpenCV_DIR /usr/share/OpenCV/)
find_package(catkin REQUIRED COMPONENTS 
  OpenCV REQUIRED
  
  roscpp
  std_msgs
  geometry_msgs
  nav_msgs
  sensor_msgs
  cv_bridge
  tf
  image_transport
  )
find_package(Sophus REQUIRED)

catkin_package()

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${Sophus_INCLUDE_DIRS}
)

set(Sophus_LIBRARIES libSophus.so)



foreach(dir config launch meshes urdf)
	install(DIRECTORY ${dir}/
		DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)

add_executable(rover_base_controller src/rover_base_controller.cpp)
target_link_libraries(rover_base_controller ${catkin_LIBRARIES})
add_executable(rover_odom_publisher src/rover_odom_publisher.cpp)
target_link_libraries(rover_odom_publisher ${catkin_LIBRARIES})
add_executable(rover_image_saver src/rover_image_saver.cpp)
target_link_libraries(rover_image_saver ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
add_executable(AHRS src/AHRS.cpp)
target_link_libraries(AHRS ${catkin_LIBRARIES} ${Sophus_LIBRARIES})

