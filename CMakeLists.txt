
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
set (CMAKE_CXX_STANDARD 17)  # Require C++14
if(COMMAND cmake_policy)
      cmake_policy(SET CMP0003 NEW)  # Required by CMake 2.7+
endif()
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.16.15.tar.gz"
    SHA1 "6974c2150fc0d3b09de3ad1efcbf15d360647ffa"
)

PROJECT(mrpt-server)

hunter_add_package(Boost COMPONENTS regex system filesystem)

FIND_PACKAGE(Boost REQUIRED regex system filesystem)
FIND_PACKAGE( MRPT REQUIRED core poses system config math nav graphslam)

# Declare the target (an executable)
ADD_EXECUTABLE(mrpt_server
	test.cpp
	)

TARGET_LINK_LIBRARIES(mrpt_server ${MRPT_LIBS} ${Boost_LIBRARIES})

# Set optimized building:
IF(CMAKE_COMPILER_IS_GNUCXX AND NOT CMAKE_BUILD_TYPE MATCHES "Debug")
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -pthread")
ENDIF()
