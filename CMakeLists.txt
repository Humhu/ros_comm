cmake_minimum_required(VERSION 2.8.3)
project(rosbag_lib)

if(NOT WIN32)
  set_directory_properties(PROPERTIES COMPILE_OPTIONS "-Wall;-Wextra")
endif()

find_package(catkin REQUIRED COMPONENTS rosbag_storage rosconsole roscpp topic_tools xmlrpcpp)
find_package(Boost REQUIRED COMPONENTS date_time regex program_options filesystem)
find_package(BZip2 REQUIRED)

catkin_python_setup()

# Support large bags (>2GB) on 32-bit systems
add_definitions(-D_FILE_OFFSET_BITS=64)

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

catkin_package(
  LIBRARIES rosbag_lib
  INCLUDE_DIRS include
  CATKIN_DEPENDS rosbag_storage rosconsole roscpp topic_tools xmlrpcpp)

add_library(rosbag_lib
  src/player.cpp
  src/recorder.cpp
  src/time_translator.cpp)

target_link_libraries(rosbag_lib ${catkin_LIBRARIES} ${Boost_LIBRARIES})

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h")
install(TARGETS rosbag_lib
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION})
