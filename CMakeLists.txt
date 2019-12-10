cmake_minimum_required(VERSION 2.8.3)
project(waypoint_maker)

find_package(autoware_build_flags REQUIRED)

find_package(
  catkin REQUIRED COMPONENTS
    amathutils_lib
    autoware_msgs
    geometry_msgs
    gnss
    lane_planner
    libwaypoint_follower
    nav_msgs
    roscpp
    std_msgs
    tf
    vector_map
)

find_package(Boost REQUIRED)

catkin_package(
  CATKIN_DEPENDS
    amathutils_lib
    autoware_msgs
    geometry_msgs
    gnss
    lane_planner
    libwaypoint_follower
    nav_msgs
    std_msgs
    tf
    vector_map
)

SET(CMAKE_CXX_FLAGS "-O2 -g -Wall ${CMAKE_CXX_FLAGS}")

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)


add_executable(waypoint_extractor nodes/waypoint_extractor/waypoint_extractor.cpp)
target_link_libraries(waypoint_extractor ${catkin_LIBRARIES})
add_dependencies(waypoint_extractor ${catkin_EXPORTED_TARGETS})





add_executable(
  waypoint_velocity_visualizer
  nodes/waypoint_velocity_visualizer/waypoint_velocity_visualizer.cpp
)

target_link_libraries(waypoint_velocity_visualizer ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(waypoint_velocity_visualizer ${catkin_EXPORTED_TARGETS})

add_executable(
  waypoint_creator
  nodes/waypoint_creator/waypoint_creator_node.cpp
  nodes/waypoint_creator/waypoint_creator_core.cpp
  nodes/waypoint_creator/interpolate.cpp
)

target_link_libraries(waypoint_creator ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(waypoint_creator ${catkin_EXPORTED_TARGETS})

install(
  TARGETS
    waypoint_velocity_visualizer
    waypoint_extractor
    waypoint_creator
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)
