
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
set (CMAKE_CXX_STANDARD 17)  # Require C++14
if(COMMAND cmake_policy)
      cmake_policy(SET CMP0003 NEW)  # Required by CMake 2.7+
endif()
INCLUDE("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.16.15.tar.gz"
    SHA1 "6974c2150fc0d3b09de3ad1efcbf15d360647ffa"
)

PROJECT(mrpt-server)

ADD_COMPILE_OPTIONS(-std=c++11)
ADD_COMPILE_OPTIONS(-pthread)
ADD_COMPILE_OPTIONS(-lboost)

HUNTER_ADD_PACKAGE(Boost COMPONENTS system coroutine)

FIND_PACKAGE(Boost CONFIG REQUIRED system regex filesystem coroutine)
FIND_PACKAGE( MRPT REQUIRED core poses system config math nav graphslam)


INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_LIST_DIR}/../${PROJECT_NAME}/include   
)

# Declare the target (an executable)
ADD_EXECUTABLE(${PROJECT_NAME}_main
	test.cpp
	)

TARGET_LINK_LIBRARIES(${PROJECT_NAME}_main ${MRPT_LIBS} ${Boost_LIBRARIES})

 INSTALL(DIRECTORY include/
 DESTINATION include)

# Set optimized building:
IF(CMAKE_COMPILER_IS_GNUCXX AND NOT CMAKE_BUILD_TYPE MATCHES "Debug")
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -pthread")
ENDIF()