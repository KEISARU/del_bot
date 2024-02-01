cmake_minimum_required(VERSION 3.5)
project(delivery_bot_package)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)

# Enable C++11/14/17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# If compiler does not support C++17, use C++14
if(NOT "${CMAKE_CXX_COMPILER_ID}" MATCHES "(GNU|Clang)")
  if(CMAKE_CXX_STANDARD STREQUAL 17)
    set(CMAKE_CXX_STANDARD 14)
  endif()
endif()

if (CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

################################################################################
# Find Dependencies
################################################################################
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(JetsonGPIO REQUIRED)

################################################################################
# Build
################################################################################
include_directories(
        include
)

set(EXECUTABLE_NAME "delivery_bot")

add_executable(${EXECUTABLE_NAME}
        src/delivery_bot.cpp
        src/DigiPot.cpp
)

set(DEPENDENCIES
        "rclcpp"
        "geometry_msgs"
)

ament_target_dependencies(${EXECUTABLE_NAME}
        ${DEPENDENCIES}
)

target_link_libraries(${EXECUTABLE_NAME} JetsonGPIO::JetsonGPIO)

install(TARGETS ${EXECUTABLE_NAME}
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
