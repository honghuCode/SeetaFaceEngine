cmake_minimum_required (VERSION 2.6)

project (viplnet)

set (VIPLNET_VERSION_MAJOR 4)
set (VIPLNET_VERSION_MINOR 5)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -std=c++11 -O2 -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -std=c++11 -O2")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4.1")
MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")

if (CMAKE_SYSTEM_NAME MATCHES "Darwin")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DGTEST_USE_OWN_TR1_TUPLE=1")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -DGTEST_USE_OWN_TR1_TUPLE=1")
endif (CMAKE_SYSTEM_NAME MATCHES "Darwin")

MESSAGE(STATUS "other platform: ${SSE3_FOUND}")
if (SSE3_FOUND)
  set (CMAKE_C_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse3")
	set (CMAKE_CXX_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse3")
	MESSAGE(STATUS "other platform: 1")
endif()
if (SSSE3_FOUND)
  set (CMAKE_C_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -mssse3")
	set (CMAKE_CXX_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse3")
	MESSAGE(STATUS "other platform: 2}")
endif()
if (SSE4_1_FOUND)
  set (CMAKE_C_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse4.1")
	set (CMAKE_CXX_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse4.1")
	MESSAGE(STATUS "other platform:3}")
endif()
if (SSE4_2_FOUND)
  set (CMAKE_C_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse4.2")
	set (CMAKE_CXX_FLAGS_ALL "${CMAKE_C_FLAGS_ALL} -msse4.2")
	MESSAGE(STATUS "other platform: 4}")
endif()


#set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} "-mssse2") 
#set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-mssse2") 


set(VIPLNET_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
set(VIPLNET_SRC_DIR ${PROJECT_SOURCE_DIR}/src)
# set __VIOL_LOG__ macro
# add_definitions(-D__VIPL_LOG__)
find_package(OpenCV 2.4.9 REQUIRED)
include_directories(${VIPLNET_INCLUDE_DIR})
include_directories(${VIPLNET_SRC_DIR})



#add_subdirectory(src)

aux_source_directory(./src SRC_LIST)
aux_source_directory(./src/classifier SRC_LIST)
aux_source_directory(./src/feat SRC_LIST)
aux_source_directory(./src/io SRC_LIST)
aux_source_directory(./src/test SRC_LIST)
aux_source_directory(./src/util SRC_LIST)
#aux_source_directory(./tools TOOLS_LIST)
add_library(viplnet STATIC ${SRC_LIST} ${TOOLS_LIST})

add_executable(detectF src/test/facedetection_test.cpp)
target_link_libraries(detectF ${OpenCV_LIBS} viplnet ${OpenCV_LIBS})
#target_link_libraries(detectF ${OpenCV_LIBS} viplnet ${OpenCV_LIBS})
#set_target_properties(detectF PROPERTIES RUNTIME_OUTPUT_DIRECTORY ../)
#set_target_properties( viplnet PROPERTIES COMPILE_FLAGS "/arch:SSE2")





#set_target_properties(viplnet PROPERTIES 
 # VERSION ${VIPLNET_VERSION_MAJOR}.${VIPLNET_VERSION_MINOR} 
 # SOVERSION ${VIPLNET_VERSION_MAJOR}.${VIPLNET_VERSION_MINOR}) 



