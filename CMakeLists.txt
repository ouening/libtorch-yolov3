# 参考链接：https://pytorch.org/cppdocs/installing.html

cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(yolo-app)

#set(OpenCV_DIR "F:/opencv/build/")
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )

# set(Torch_DIR "E:/MachineLearning/libtorch/share/cmake/Torch")
# set(CMAKE_PREFIX_PATH "E:/Downloads/libtorch")
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

# The following code block is suggested to be used on Windows.
# According to https://github.com/pytorch/pytorch/issues/25457,
# the DLLs need to be copied to avoid memory errors.
if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET yolo-app
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:yolo-app>)
endif (MSVC)