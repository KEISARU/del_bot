cmake_minimum_required(VERSION 3.5)
project(digipot)

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

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(JetsonGPIO REQUIRED)
find_package(digipot_interfaces REQUIRED)

add_executable(digipot src/digipot.cpp src/DigiPot.cpp)
target_include_directories(digipot PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(digipot PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(
  digipot
  "rclcpp"
  "std_msgs"
  "geometry_msgs"
  "sensor_msgs"
  "digipot_interfaces"
)
target_link_libraries(digipot JetsonGPIO::JetsonGPIO)

install(TARGETS digipot
  DESTINATION lib/${PROJECT_NAME})

# Install Python executables
install(PROGRAMS
  scripts/digipot_node.py
  DESTINATION lib/${PROJECT_NAME}
)

# Install launch files
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}/
)

# Install Python modules
ament_python_install_package(${PROJECT_NAME})

# Install package.xml
install(FILES
  package.xml
  DESTINATION share/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
