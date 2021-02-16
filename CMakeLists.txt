cmake_minimum_required(VERSION 2.8.3)
project(or_urdf)

list(INSERT CMAKE_MODULE_PATH 0 "${PROJECT_SOURCE_DIR}/cmake") 

find_package(catkin REQUIRED COMPONENTS
    roslib
    srdfdom
    urdf
    openrave_catkin
)

find_package(Boost REQUIRED COMPONENTS
    filesystem
    system
)
find_package(OpenRAVE REQUIRED)
find_package(TinyXML2 REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

catkin_package()

include_directories(
    ${Boost_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}
    ${TinyXML2_INCLUDE_DIRS}
)
link_directories(
    ${Boost_LIBRARY_DIRS}
    ${catkin_LIBRARY_DIRS}
)

openrave_plugin("${PROJECT_NAME}_plugin"
    src/urdf_loader.cpp
    src/catkin_finder.cpp
)

target_link_libraries("${PROJECT_NAME}_plugin"
    ${catkin_LIBRARIES}
    ${TinyXML2_LIBRARIES}
)

if(CATKIN_ENABLE_TESTING)
    catkin_add_nosetests(tests)
endif(CATKIN_ENABLE_TESTING)


