cmake_minimum_required(VERSION 3.8)


message(STATUS "\n\n\n\n CMakeLists de Viewer global \n\n\n\n")

#project(viewerdpvox)
project(viewerdpvox LANGUAGES CUDA CXX)

add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_CUDA_STANDARD 17)


message(STATUS "\n\n nvcc arch : ${CUDA_NVCC_FLAGS} \n\n")


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

#set(CMAKE_CUDA_FLAGS "${CMAKE_CXX_FLAGS}")
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -O3")


message(STATUS "\n\n nvcc arch 2 : ${CUDA_NVCC_FLAGS} \n\n")


#set(CMAKE_CUDA_ARCHITECTURES 89)
set(CMAKE_CUDA_ARCHITECTURES "80;86;89")


message(STATUS "\n\n nvcc arch 3 : ${CUDA_NVCC_FLAGS} \n\n")


#set(CUDA_ARCH_BIN "75 80 86 89")

# include(FindCUDA/select_compute_arch)
# CUDA_DETECT_INSTALLED_GPUS(INSTALLED_GPU_CCS_1)
# string(STRIP "${INSTALLED_GPU_CCS_1}" INSTALLED_GPU_CCS_2)
# string(REPLACE " " ";" INSTALLED_GPU_CCS_3 "${INSTALLED_GPU_CCS_2}")
# string(REPLACE "." "" CUDA_ARCH_LIST "${INSTALLED_GPU_CCS_3}")
# SET(CMAKE_CUDA_ARCHITECTURES ${CUDA_ARCH_LIST})


list(APPEND CMAKE_PREFIX_PATH ${TORCH_PATH})

find_package(Torch REQUIRED)
find_library(TORCH_PYTHON_LIBRARY torch_python PATHS "${TORCH_INSTALL_PREFIX}/lib")

include_directories(
    ${EIGEN_INCLUDE_DIRS}
    ${TORCH_INCLUDE_DIRS} 
) 

# Définir les options spécifiques pour nvcc
set(CUDA_NVCC_FLAGS
    -O3
    -gencode=arch=compute_80,code=sm_80
    -gencode=arch=compute_86,code=sm_86
    -gencode=arch=compute_86,code=sm_89
)

set(CMAKE_CUDA_ARCHITECTURES "80;86;89")

message(STATUS "nvcc arch : ${CUDA_NVCC_FLAGS}")
message(STATUS "cuda arch : ${CMAKE_CUDA_ARCHITECTURES}")

# pybind11
add_subdirectory(pybind11)

# viewer
add_subdirectory(viewerdpvo)



