############################################################################
# <summary> CMakeLists.txt for OpenCV and CUDA. </summary>
# <date>    2012-02-07          </date>
# <author>  Quan Tran Minh. edit by Johannes Kast, Michael Sarahan </author>
# <email>   quantm@unist.ac.kr  kast.jo@googlemail.com msarahan@gmail.com</email>
############################################################################

cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(cs344)

find_package(OpenCV REQUIRED)
find_package(CUDA REQUIRED)

link_libraries(${OpenCV_LIBS} )

get_filename_component(PARENT_DIR ${CMAKE_SOURCE_DIR} PATH)

set (EXECUTABLE_OUTPUT_PATH "${PARENT_DIR}/cs344-project/bin")

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    if(NOT CMAKE_C_COMPILER_VERSION VERSION_LESS 5.4)
        SET(CMAKE_C_COMPILER /opt/rh/devtoolset-3/root/bin/gcc)
        SET(CMAKE_CXX_COMPILER /opt/rh/devtoolset-3/root/bin/g++)
        SET(CUDA_HOST_COMPILER ${CMAKE_C_COMPILER})
    endif()
endif()

if(CUDA_FOUND)
    # compared to class settings, we let NVidia's FindCUDA CMake detect
    # whether to build x64.  We tell it to support most devices, though,
    # to make sure more people can easily run class code without knowing
    # about this compiler argument
    # pascal arch
    set(CUDA_NVCC_FLAGS "-gencode arch=compute_61,code=sm_61;")
    # fermi arch
    set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} "-gencode=arch=compute_20,code=sm_20;")

    # add -Wextra compiler flag for gcc compilations
    if (UNIX)
	set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} "-Xcompiler -Wall;")
	set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} "-Xcompiler -Wextra")
	set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} "-Xcompiler -g")
    endif (UNIX)

    # add debugging to CUDA NVCC flags.  For NVidia's NSight tools.
    set(CUDA_NVCC_FLAGS_DEBUG ${CUDA_NVCC_FLAGS_DEBUG} "-G")

    add_subdirectory (HW1)
    add_subdirectory (HW2)
    add_subdirectory (HW3)
    add_subdirectory (HW4)
    add_subdirectory (HW5)
    add_subdirectory (HW6)
else(CUDA_FOUND)
    message("CUDA is not installed on this system.")
endif()
