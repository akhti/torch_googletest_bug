cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(torch_test)

# Build GTest.
# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Add googletest directly to our build. This defines
# the gtest and gtest_main targets.
add_subdirectory(${CMAKE_SOURCE_DIR}/third_party/googletest third_party/googletest EXCLUDE_FROM_ALL)

execute_process(
  COMMAND python -c "import torch; import os; print(os.path.dirname(torch.__file__), end='')"
  OUTPUT_VARIABLE TorchPath
)
list(APPEND CMAKE_PREFIX_PATH ${TorchPath})

# set cuda arch list so that the built binary can be run on both pascal and volta
set(TORCH_CUDA_ARCH_LIST "6.0;7.0")

# find packages & third_party
find_package(PythonInterp 3.7 REQUIRED)
find_package(PythonLibs 3.7 REQUIRED)
find_package(Torch REQUIRED)

add_executable(test_simple_notorch simple_test.cc)
target_link_libraries(test_simple_notorch gtest_main)

add_executable(test_simple simple_test.cc)
target_link_libraries(test_simple "${TORCH_LIBRARIES}" gtest_main)
