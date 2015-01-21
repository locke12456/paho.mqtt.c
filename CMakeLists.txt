cmake_minimum_required(VERSION 2.8)

project(paho-mqtt)

#set(CMAKE_BUILD_TYPE Debug)
#SET(BUILD_STATIC)

INCLUDE(os_env.cmake)

set(RELEASE_DIR ${CMAKE_SOURCE_DIR}/paho_mqtt_c_release)
# Set common include directories
include_directories(${CMAKE_SOURCE_DIR}/libraries/include)
link_directories(${LIBS_PATH}/openssl/static)
ADD_SUBDIRECTORY(paho-mqtt3c)
ADD_SUBDIRECTORY(paho-mqtt3cs)
ADD_SUBDIRECTORY(paho-mqtt3a)
ADD_SUBDIRECTORY(paho-mqtt3as)

