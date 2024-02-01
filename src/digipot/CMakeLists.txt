cmake_minimum_required(VERSION 3.5)
project(delivery_bot_package)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)

# Add other necessary dependencies if needed

# Include directories
include_directories(
        include
)

# Build executable
add_executable(delivery_bot
        src/delivery_bot.cpp
        src/DigiPot.cpp
)

# Link against dependencies
ament_target_dependencies(delivery_bot
        "rclcpp"
        "std_msgs"
        "geometry_msgs"
        "sensor_msgs"
        # Add other dependencies here if needed
)

# Install executable
install(TARGETS delivery_bot
        DESTINATION lib/${PROJECT_NAME})

# Install launch files
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}/
)

# Install Python modules (if any)
# ament_python_install_package(${PROJECT_NAME})

# Install package.xml
install(FILES
  package.xml
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
