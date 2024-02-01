cmake_minimum_required(VERSION 3.5)
project(digipot_interfaces)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# Generate ROS interfaces
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/Digipot.msg"
)

# If you want to enable testing
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # The following line skips the linter that checks for copyrights.
  # Comment the line when a copyright and license are added to all source files.
  set(ament_cmake_copyright_FOUND TRUE)
  # The following line skips cpplint (only works in a git repo).
  # Comment the line when this package is in a git repo and when
  # a copyright and license are added to all source files.
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

# Install Python executables
install(PROGRAMS
  scripts/digipot_interface_node.py
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

ament_package()
