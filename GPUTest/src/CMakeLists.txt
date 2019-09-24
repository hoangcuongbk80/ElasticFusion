cmake_minimum_required(VERSION 2.6.0)

project(GPUTest)

find_package(Pangolin 0.1 REQUIRED)
find_package(CUDA REQUIRED)

find_package(OpenCV 3.4.0 REQUIRED NO_MODULE PATHS /usr/local NO_DEFAULT_PATH)
if (OpenCV_INCLUDE_DIRS)
    include_directories("${OpenCV_INCLUDE_DIRS}")
    link_directories(${OpenCV_DIR}/lib)
    message(STATUS "OpenCV library status:")
    message(STATUS "    version: ${OpenCV_VERSION}")
    message(STATUS "    libraries: ${OpenCV_LIBS}")
    message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")
endif (OpenCV_INCLUDE_DIRS)

set(efusion_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../Core/src" CACHE PATH "Where ElasticFusion.h lives")
set(efusion_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/../../Core/build/libefusion.so" CACHE FILEPATH "Where libefusion.so lives")

include_directories(/usr/include/suitesparse)
include_directories(${CUDA_INCLUDE_DIRS})
include_directories(${EIGEN_INCLUDE_DIRS})
include_directories(${Pangolin_INCLUDE_DIRS})
include_directories(${efusion_INCLUDE_DIR})

file(GLOB srcs *.cpp)

set(CMAKE_CXX_FLAGS "-O3 -msse2 -msse3 -Wall -std=c++11")
#set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} " -g -Wall")

add_executable(GPUTest 
               ${srcs}
)

target_link_libraries(GPUTest
                      ${efusion_LIBRARY}
                      ${Eigen_LIBRARIES}
                      ${Pangolin_LIBRARIES}
                      ${CUDA_LIBRARIES}
                      ${SUITESPARSE_LIBRARIES}
                      ${OpenCV_LIBS}
)

