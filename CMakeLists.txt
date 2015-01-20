cmake_minimum_required(VERSION 2.8)

project(paho-mqtt)

set(CMAKE_BUILD_TYPE Debug)

INCLUDE(os_env.cmake)

# Set common include directories
include_directories(${CMAKE_SOURCE_DIR}/libraries/include)
link_directories(${LIBS_PATH}/openssl)
ADD_SUBDIRECTORY(paho-mqtt3c)
ADD_SUBDIRECTORY(paho-mqtt3cs)
ADD_SUBDIRECTORY(paho-mqtt3a)
ADD_SUBDIRECTORY(paho-mqtt3as)

