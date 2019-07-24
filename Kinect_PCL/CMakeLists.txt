cmake_minimum_required(VERSION 3.0.0)

project(Kinect_PCL VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)


find_package(PCL 1.2 REQUIRED)
find_package(libfreenect REQUIRED)


include_directories(${PCL_INCLUDE_DIRS})
include_directories("/usr/include/libusb-1.0/")

link_directories(${PCL_LIBRARY_DIRS})

add_definitions(${PCL_DEFINITIONS})

add_executable(${PROJECT_NAME} "main.cpp")


target_link_libraries (${PROJECT_NAME} ${PCL_LIBRARIES}
                                       ${FREENECT_LIBRARIES})
