project(paho-mqtt)

set(srcdir ${CMAKE_SOURCE_DIR}/src)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${RELEASE_DIR}")
add_definitions(-DOPENSSL)
add_definitions(-DWIN32_LEAN_AND_MEAN)

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
file(GLOB_RECURSE SOURCE_FILES_CS
	${srcdir}/MQTTVersion.c 
	${srcdir}/MQTTClient.c
	${srcdir}/MQTTPacket.c
	${srcdir}/MQTTPacketOut.c
	${srcdir}/MQTTPersistence.c
	${srcdir}/MQTTPersistenceDefault.c
	${srcdir}/MQTTProtocolClient.c
	${srcdir}/MQTTProtocolOut.c
)

include_directories(${srcdir})
IF(DEFINED BUILD_STATIC)
	ADD_LIBRARY(paho-mqtt3cs STATIC
		${SOURCE_FILES}
		${SOURCE_FILES_CS}
	)
ELSE()
	ADD_LIBRARY(paho-mqtt3cs SHARED
		${SOURCE_FILES}
		${SOURCE_FILES_CS}
	)
endif()

if(UNIX)
    target_link_libraries(paho-mqtt3cs pthread)
    target_link_libraries(paho-mqtt3cs dl)

    target_link_libraries(paho-mqtt3cs ssl)
    target_link_libraries(paho-mqtt3cs crypto)
endif(UNIX)

if(WIN32)
	target_link_libraries(paho-mqtt3cs ws2_32)
    target_link_libraries(paho-mqtt3cs libeay32)
	target_link_libraries(paho-mqtt3cs ssleay32)
endif(WIN32)

# Copy targets to release folder
add_custom_command(
	TARGET paho-mqtt3cs POST_BUILD 
	COMMAND ${CMAKE_COMMAND} -E copy 
		$<TARGET_FILE:paho-mqtt3cs>
		${RELEASE_DIR}
	COMMENT "Copying dynamic linker to output directory"
)
