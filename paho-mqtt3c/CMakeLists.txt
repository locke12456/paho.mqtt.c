project(paho-mqtt)


set(srcdir ${CMAKE_SOURCE_DIR}/src)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${RELEASE_DIR}")
#SET( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS}  -R -Wl,--dynamic-linker=/usr/arm-linux-gnueabihf/lib/ -ldl " )
#SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -R -Wl,--dynamic-linker=/usr/arm-linux-gnueabihf/lib/ -ldl " )
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
file(GLOB_RECURSE SOURCE_FILES_C
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
	ADD_LIBRARY(paho-mqtt3c STATIC
		${SOURCE_FILES}
		${SOURCE_FILES_C}
	)
ELSE()
	ADD_LIBRARY(paho-mqtt3c SHARED
		${SOURCE_FILES}
		${SOURCE_FILES_C}
	)

endif()

if(UNIX)
    target_link_libraries(paho-mqtt3c pthread)
    target_link_libraries(paho-mqtt3c dl)
    target_link_libraries(paho-mqtt3c ssl)
    target_link_libraries(paho-mqtt3c crypto)
endif(UNIX)

if(WIN32)
	target_link_libraries(paho-mqtt3c ws2_32)
    target_link_libraries(paho-mqtt3c libeay32)
    target_link_libraries(paho-mqtt3c ssleay32)
endif(WIN32)


add_custom_command(
	TARGET paho-mqtt3c POST_BUILD 
	COMMAND ${CMAKE_COMMAND} -E copy 
		$<TARGET_FILE:paho-mqtt3c>
		${RELEASE_DIR}
	COMMENT "Copying dynamic linker to output directory"
)
