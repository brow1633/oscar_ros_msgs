cmake_minimum_required(VERSION 3.8)
project(oscar_ros_msgs)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

macro(subdirlist result dir type)
  file(GLOB children "${dir}/${type}")
  set(filelist "")
  foreach(child ${children})
      get_filename_component(child ${child} NAME)
      list(FIND EXCEPTION_MSG ${child} index)
      if(${index} EQUAL -1)
        list(APPEND filelist "${dir}/${child}")
      else()
        message("${child} is not supported in ros2")
      endif()
  endforeach()
  set(${result} ${filelist})
endmacro()

subdirlist(MSG_FILES "msg" "*.msg")

rosidl_generate_interfaces(${PROJECT_NAME}
    ${MSG_FILES}
    DEPENDENCIES std_msgs
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()
