cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(yolo-app)

#set(OpenCV_DIR "F:/opencv/build/")
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )

set(Torch_DIR "E:/MachineLearning/libtorch/share/cmake/Torch")
find_package(Torch REQUIRED)

if(NOT Torch_FOUND)
    message(FATAL_ERROR "Pytorch Not Found!")
endif(NOT Torch_FOUND)

message(STATUS "Pytorch status:")
message(STATUS "    libraries: ${TORCH_LIBRARIES}")

message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

aux_source_directory(. DIR_SRCS)

add_executable(yolo-app ${DIR_SRCS})
target_link_libraries(yolo-app "${TORCH_LIBRARIES}" "${OpenCV_LIBS}")
set_property(TARGET yolo-app PROPERTY CXX_STANDARD 11)
