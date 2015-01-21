project(paho-mqtt)

set(srcdir ${CMAKE_SOURCE_DIR}/src)

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${RELEASE_DIR}")

file(GLOB_RECURSE SOURCE_FILES
	${srcdir}/Clients.c
	${srcdir}/Heap.c
	${srcdir}/LinkedList.c
	${srcdir}/Log.c
	${srcdir}/Messages.c
	${srcdir}/Socket.c
	${srcdir}/SocketBuffer.c
	${srcdir}/SSLSocket.c
	${srcdir}/StackTrace.c
	${srcdir}/Thread.c
	${srcdir}/Tree.c
	${srcdir}/utf-8.c
)
file(GLOB_RECURSE SOURCE_FILES_A
	${srcdir}/*.h
	${srcdir}/MQTTVersion.c 
	${srcdir}/MQTTAsync.c
	${srcdir}/MQTTPacket.c
	${srcdir}/MQTTPacketOut.c
	${srcdir}/MQTTPersistence.c
	${srcdir}/MQTTPersistenceDefault.c
	${srcdir}/MQTTProtocolClient.c
	${srcdir}/MQTTProtocolOut.c
)

include_directories(${srcdir})

IF(DEFINED BUILD_STATIC)
	ADD_LIBRARY(paho-mqtt3a STATIC
		${SOURCE_FILES_A}
		${SOURCE_FILES}
	)
ELSE()
	ADD_LIBRARY(paho-mqtt3a SHARED
		${SOURCE_FILES_A}
		${SOURCE_FILES}
	)
endif()


if(UNIX)
    target_link_libraries(paho-mqtt3a ssl)
    target_link_libraries(paho-mqtt3a crypto)
endif(UNIX)

if(WIN32)
	target_link_libraries(paho-mqtt3a ws2_32)
    target_link_libraries(paho-mqtt3a libeay32)
    target_link_libraries(paho-mqtt3a ssleay32)
endif(WIN32)
# Copy targets to release folder
add_custom_command(
	TARGET paho-mqtt3a POST_BUILD 
	COMMAND ${CMAKE_COMMAND} -E copy 
		$<TARGET_FILE:paho-mqtt3a>
		${RELEASE_DIR}
	COMMENT "Copying dynamic linker to output directory"
)