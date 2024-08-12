cmake_minimum_required(VERSION 3.29)
project(kaleidoscope_llvm)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_CXX_COMPILER "clang++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -o3 -rdynamic")

find_package(LLVM 20 REQUIRED CONFIG)
message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

include_directories(${LLVM_INCLUDE_DIRS})
separate_arguments(LLVM_DEFINITIONS_LIST NATIVE_COMMAND ${LLVM_DEFINITIONS})
add_definitions(${LLVM_DEFINITIONS_LIST})

include_directories(.)

add_executable(kaleidoscope_llvm
        KaleidoscopeJIT.h
        toy.cpp)

llvm_map_components_to_libnames(llvm_libs core orcjit native)

target_link_libraries(kaleidoscope_llvm ${llvm_libs})