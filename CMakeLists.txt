cmake_minimum_required(VERSION 3.5)
project(sensor_fusion)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(zed_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)

find_package(OpenCV REQUIRED)
find_package(cv_bridge REQUIRED)

find_package(yaml-cpp REQUIRED)
include_directories(${YAMLCPP_INCLUDE_DIRS})

find_package(PCL 1.3 REQUIRED COMPONENTS common io conversion)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

include_directories(include/sensor_fusion)

add_executable(sensor_fusion_node src/main.cpp src/SensorFusionNode.cpp)
target_link_libraries (sensor_fusion_node ${PCL_LIBRARIES} yaml-cpp)

ament_target_dependencies(sensor_fusion_node
  rclcpp
  OpenCV
  cv_bridge
  yaml-cpp
  zed_msgs
  sensor_msgs
)
        
install(DIRECTORY launch config
  DESTINATION share/${PROJECT_NAME}
)

install(DIRECTORY include
  DESTINATION include/${PROJECT_NAME}
)

install(DIRECTORY config
  DESTINATION share/${PROJECT_NAME}
)

install(TARGETS sensor_fusion_node
        DESTINATION lib/${PROJECT_NAME})

ament_package()
