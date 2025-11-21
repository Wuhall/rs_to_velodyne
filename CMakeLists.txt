cmake_minimum_required(VERSION 3.8)
project(rs_to_velodyne)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(message_filters REQUIRED)

# Set CMake policy for PCL_ROOT
if(POLICY CMP0074)
  cmake_policy(SET CMP0074 NEW)
endif()
find_package(PCL REQUIRED)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(
  include
  ${PCL_INCLUDE_DIRS}
)

link_directories(
  ${PCL_LIBRARY_DIRS}
)

add_executable(rs_to_velodyne src/rs_to_velodyne.cpp)
ament_target_dependencies(rs_to_velodyne
  rclcpp
  sensor_msgs
  std_msgs
  pcl_conversions
  pcl_ros
  message_filters
)
target_link_libraries(rs_to_velodyne ${PCL_LIBRARIES})

install(TARGETS
  rs_to_velodyne
  DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
