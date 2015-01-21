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
file(GLOB_RECURSE SOURCE_FILES_AS
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
	ADD_LIBRARY(paho-mqtt3as STATIC
		${SOURCE_FILES_AS}
		${SOURCE_FILES}
	)
ELSE()
	ADD_LIBRARY(paho-mqtt3as SHARED
		${SOURCE_FILES_AS}
		${SOURCE_FILES}
	)
endif()


if(UNIX)
    target_link_libraries(paho-mqtt3as ssl)
    target_link_libraries(paho-mqtt3as crypto)
endif(UNIX)

if(WIN32)
	target_link_libraries(paho-mqtt3as ws2_32)
    target_link_libraries(paho-mqtt3as libeay32)
	target_link_libraries(paho-mqtt3as ssleay32)
endif(WIN32)

