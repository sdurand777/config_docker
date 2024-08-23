
# optional libraries
find_package(Pangolin REQUIRED)

message(STATUS "\n\n\n\n CMakeLists de viewerdpvo \n\n\n\n")


# # Avant de configurer les architectures CUDA, nettoyez toutes les valeurs précédentes
# unset(CMAKE_CUDA_ARCHITECTURES CACHE)
# unset(CUDA_NVCC_FLAGS CACHE)
#
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
#
# # Set nvcc flags (optional if you want to add additional flags)
# set(CUDA_NVCC_FLAGS
#     -O3
#     -gencode=arch=compute_80,code=sm_80
#     -gencode=arch=compute_86,code=sm_86
#     -gencode=arch=compute_86,code=sm_89
# )
#
# set(CMAKE_CUDA_ARCHITECTURES "80;86;89")
# Create a shared library (can be a static library as well)

# Reset CUDA architectures and flags
unset(CMAKE_CUDA_ARCHITECTURES CACHE)
unset(CUDA_NVCC_FLAGS CACHE)

# Set desired CUDA architectures
set(CMAKE_CUDA_ARCHITECTURES "80;86;89")

# # Define CUDA NVCC flags
# set(CUDA_NVCC_FLAGS "-O3")
# list(APPEND CUDA_NVCC_FLAGS
#     "-gencode=arch=compute_80,code=sm_80"
#     "-gencode=arch=compute_86,code=sm_86"
#     "-gencode=arch=compute_89,code=sm_89"
# )

# Define CUDA NVCC flags correctly
set(CUDA_NVCC_FLAGS "")
list(APPEND CUDA_NVCC_FLAGS "-O3")
list(APPEND CUDA_NVCC_FLAGS "--generate-code=arch=compute_89,code=[compute_89,sm_89]")

string(REPLACE ";" " " CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS}")

# Ensure CUDA flags are set
set(CMAKE_CUDA_FLAGS "${CUDA_NVCC_FLAGS}")






message(STATUS "\n\n\n\n ADD LIBRARY \n\n\n\n")

add_library(viewerdpvo SHARED viewer.cpp viewer_cuda.cu)

# Récupérer les options de compilation
get_target_property(COMPILE_DEFINITIONS viewerdpvo COMPILE_DEFINITIONS)



include_directories(${Pangolin_INCLUDE_DIRS})

# Apply CUDA architectures to the created target
set_property(TARGET viewerdpvo PROPERTY CUDA_ARCHITECTURES 80 86)

# Link libraries (including pybind11 manually)
set_target_properties(viewerdpvo PROPERTIES CUDA_ARCHITECTURES "80;86")


message(STATUS "\n\n\n\n CREATE MODULE \n\n\n\n")

message(STATUS "nvcc arch : ${CUDA_NVCC_FLAGS}")

message(STATUS "cuda arch : ${CMAKE_CUDA_ARCHITECTURES}")

message(STATUS "cuda flags : ${CMAKE_CUDA_FLAGS}")

target_link_libraries(viewerdpvo PRIVATE ${TORCH_LIBRARIES} ${TORCH_PYTHON_LIBRARY} ${Pangolin_LIBRARIES})

target_link_libraries(viewerdpvo PRIVATE pybind11::module)  # Pybind11 lié directement ici

target_include_directories(viewerdpvo PRIVATE ${pybind11_INCLUDE_DIRS})

# # Add Python bindings using pybind11
# pybind11_add_module(viewerdpvo_python_binding MODULE viewerdpvo)

# Appliquer les architectures CUDA à la cible viewerdpvo





