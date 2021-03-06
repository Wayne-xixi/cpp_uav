cmake_minimum_required(VERSION 2.8.3)
project(cpp_uav)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  rospy
  std_msgs
  message_generation
  roslint
)

find_package(CGAL REQUIRED COMPONENTS Core)

include( ${CGAL_USE_FILE} )
include( CGAL_CreateSingleSourceCGALProgram )

add_service_files(
  FILES
  Torres16.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)

catkin_package(
  CATKIN_DEPENDS message_runtime std_msgs geometry_msgs
)

catkin_package()

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

# lint
roslint_cpp()
roslint_python()

# Executables
add_executable(torres_etal_2016 src/torres_etal_2016.cpp)

add_dependencies(torres_etal_2016 ${PROJECT_NAME}_generate_messages_cpp)

target_link_libraries(torres_etal_2016 ${catkin_LIBRARIES})

# Tests
catkin_add_gtest(test_torres_etal_2016 test/test_torres_etal_2016.cpp)
catkin_add_gtest(test_cgutil test/test_cgutil.cpp)

target_link_libraries(test_torres_etal_2016 ${catkin_LIBRARIES})
target_link_libraries(test_cgutil ${catkin_LIBRARIES})