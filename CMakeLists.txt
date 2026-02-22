cmake_minimum_required(VERSION 3.8)
project(ros2_custom_msgs)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# Generate custom message
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/RobotStatus.msg"
)

# Add executable
add_executable(status_publisher src/status_publisher.cpp)

# Add standard dependencies
ament_target_dependencies(status_publisher
  rclcpp
  std_msgs
)

# --- FIX START: New way to link custom messages in ROS 2 Jazzy ---
rosidl_get_typesupport_target(cpp_typesupport_target "${PROJECT_NAME}" "rosidl_typesupport_cpp")
target_link_libraries(status_publisher "${cpp_typesupport_target}")
# --- FIX END ---

# Install the executable so "ros2 run" can find it
install(TARGETS
  status_publisher
  DESTINATION lib/${PROJECT_NAME}
)

# Export dependencies for other packages
ament_export_dependencies(rosidl_default_runtime)

ament_package()
