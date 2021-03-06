cmake_minimum_required (VERSION 2.6)

project (viplnet)

set (VIPLNET_VERSION_MAJOR 4)
set (VIPLNET_VERSION_MINOR 5)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -std=c++11 -O2 -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -std=c++11 -O2")

option(USE_OPENMP      "Set to ON to build use openmp"  ON)
option(USE_SSE         "Set to ON to build use SSE"  ON)

MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")

if (CMAKE_SYSTEM_NAME MATCHES "Darwin")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DGTEST_USE_OWN_TR1_TUPLE=1")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -DGTEST_USE_OWN_TR1_TUPLE=1")
endif (CMAKE_SYSTEM_NAME MATCHES "Darwin")

set(VIPLNET_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
set(VIPLNET_SRC_DIR ${PROJECT_SOURCE_DIR}/src)
# set __VIOL_LOG__ macro
# add_definitions(-D__VIPL_LOG__)

include_directories(${VIPLNET_INCLUDE_DIR})
include_directories(${VIPLNET_SRC_DIR})
include_directories(/usr/include)
include_directories(../FaceDetection/include)
include_directories(../FaceAlignment/include)
link_directories(../FaceDetection/build)
link_directories(../FaceAlignment/build)

add_subdirectory(src)
add_subdirectory(src/sftest)
aux_source_directory(./src SRC_LIST)
aux_source_directory(./tools TOOLS_LIST)
add_library(viplnet SHARED ${SRC_LIST} ${TOOLS_LIST} )
target_link_libraries(
    viplnet
    openblas
)
set_target_properties(viplnet PROPERTIES 
  VERSION ${VIPLNET_VERSION_MAJOR}.${VIPLNET_VERSION_MINOR} 
  SOVERSION ${VIPLNET_VERSION_MAJOR}.${VIPLNET_VERSION_MINOR}) 
