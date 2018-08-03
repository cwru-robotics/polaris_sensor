cmake_minimum_required(VERSION 2.8.3)
project(polaris_sensor)

find_package(Eigen3)
set(Eigen3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
set(Eigen3_DEP Eigen3)
add_definitions(${EIGEN_DEFINITIONS})

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  sensor_msgs
  serial
  cmake_modules
  message_generation
  cwru_msgs
  xform_utils
)

add_message_files(
  FILES StrayMarker.msg
)
generate_messages(
  DEPENDENCIES std_msgs geometry_msgs
)

#list( APPEND CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS} -g -ftest-coverage -fprofile-arcs")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
## System dependencies are found with CMake's conventions
find_package(Boost COMPONENTS system filesystem REQUIRED)

catkin_package(
  DEPENDS ${Eigen3_DEP}
  INCLUDE_DIRS include ${EIGEN3_INCLUDE_DIRS}
  LIBRARIES polaris_sensor
  CATKIN_DEPENDS geometry_msgs roscpp serial sensor_msgs message_runtime cwru_msgs xform_utils
)

include_directories(include)
include_directories(
  include
  ${Boost_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
  ${Eigen3_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
   src/polaris_sensor.cpp
)

add_executable(${PROJECT_NAME}_node src/polaris_ros.cpp)

target_link_libraries(${PROJECT_NAME}
   ${catkin_LIBRARIES}
   ${Boost_LIBRARIES}
)

target_link_libraries(${PROJECT_NAME}_node
   ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${PROJECT_NAME}
)


install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
