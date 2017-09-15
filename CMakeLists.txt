cmake_minimum_required(VERSION 3.8)
project(caffeclassification)
set(CMAKE_CXX_STANDARD 11)

add_definitions(-DUSE_OPENCV -DCPU_ONLY)
include_directories(/home/lin/dl/caffe/include)

# Find OpenCV, you may need to set OpenCV_DIR variable
# to the absolute path to the directory containing OpenCVConfig.cmake file
# via the command line or GUI
find_package(OpenCV REQUIRED)

# If the package has been found, several variables will
# be set, you can find the full list with descriptions
# in the OpenCVConfig.cmake file.
# Print some message showing some of them
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

# Add OpenCV headers location to your include paths
include_directories(${OpenCV_INCLUDE_DIRS})
list(APPEND LINKER_LIBS ${OpenCV_LIBS})

# glog
include("cmake/FindGlog.cmake")
include_directories(SYSTEM ${GLOG_INCLUDE_DIRS})
list(APPEND LINKER_LIBS ${GLOG_LIBRARIES})

# gflags
include("cmake/FindGFlags.cmake")
#find_package(GFlags REQUIRED)
include_directories(SYSTEM ${GFLAGS_INCLUDE_DIRS})
list(APPEND LINKER_LIBS ${GFLAGS_LIBRARIES})

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})

set(SOURCE_FILES main.cpp)
add_executable(caffeclassification ${SOURCE_FILES})
target_link_libraries(caffeclassification ${LINKER_LIBS}
        /home/lin/dl/caffe/build/lib/libcaffe.so
        libboost_system.so
        libboost_thread.so
        )