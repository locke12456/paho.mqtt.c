cmake_minimum_required(VERSION 2.8)

project(paho-mqtt)



#set(CMAKE_BUILD_TYPE Debug)
#SET(BUILD_STATIC)

#include(toolchain-arm-linux-gnueabi.cmake)

INCLUDE(os_env.cmake)


set(RELEASE_DIR ${CMAKE_SOURCE_DIR}/paho_mqtt_c_release)
# Set common include directories
include_directories(${CMAKE_SOURCE_DIR}/libraries/include)
link_directories(${LIBS_PATH}/openssl)
ADD_SUBDIRECTORY(paho-mqtt3c)
ADD_SUBDIRECTORY(paho-mqtt3cs)
ADD_SUBDIRECTORY(paho-mqtt3a)
ADD_SUBDIRECTORY(paho-mqtt3as)

