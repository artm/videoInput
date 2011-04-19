PROJECT(videoInput)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

ADD_LIBRARY(videoInput videoInput.cpp)

IF(MSYS OR MINGW)
	INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/Include/BaseClasses)
	INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/Include/DShow)

	SET_TARGET_PROPERTIES(videoInput PROPERTIES 
						  COMPILE_FLAGS "-include ${CMAKE_SOURCE_DIR}/unvc.h -Wno-multichar")
ELSE()
    # find headers from vista sdk
    INCLUDE_DIRECTORIES(C:/Program\ Files/Microsoft\ SDKs/Windows/v6.0/VC/Include)
    INCLUDE_DIRECTORIES(C:/Program\ Files/Microsoft\ SDKs/Windows/v6.0/VC/Include/Sys)
    # the order is crucial - microsofts left hemisphere doesn't know what the right one is doing
    # and they end up creating source trees with conflicting branches
    INCLUDE_DIRECTORIES(C:/Program\ Files/Microsoft\ SDKs/Windows/v6.0/Samples/Multimedia/DirectShow/BaseClasses)
    INCLUDE_DIRECTORIES(C:/Program\ Files/Microsoft\ SDKs/Windows/v6.0/Include)
    # and old direct show 9 headers
	INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/Include/DShow.orig0)
    SET_TARGET_PROPERTIES(videoInput PROPERTIES
                          COMPILE_FLAGS "/wd4995 /wd4244 /wd4996")
ENDIF()