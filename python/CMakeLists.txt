cmake_minimum_required(VERSION 3.8)

# Add the happy Python extension target.
add_library(happy_ext MODULE happy_ext.cpp)
set_property(TARGET happy_ext PROPERTY POSITION_INDEPENDENT_CODE ON)
target_compile_features(happy_ext PUBLIC cxx_std_14)
if(CMAKE_CUDA_COMPILER)
  target_compile_definitions(happy_ext PRIVATE WITH_CUDA)
endif()

# Link to dependencies.
target_link_libraries(happy_ext PRIVATE pybind11::module happy_lib)

# Configure the built library to be consumed as a Python extension.
set_target_properties(
  happy_ext
  PROPERTIES
    PREFIX "${PYTHON_MODULE_PREFIX}"
    SUFFIX "${PYTHON_MODULE_EXTENSION}"
    OUTPUT_NAME "happy"
)
