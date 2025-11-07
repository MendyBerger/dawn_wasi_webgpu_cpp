cmake_minimum_required(VERSION 3.14)
project(dawn_webgpu_cpp VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Fetch wasi-webgpu-headers
include(FetchContent)

FetchContent_Declare(
    wasi_webgpu_headers
    GIT_REPOSITORY https://github.com/wasi-gfx/wasi-webgpu-headers.git
    GIT_TAG b1584349095e1a378c60bad99cac23749bb34fe2
)

FetchContent_GetProperties(wasi_webgpu_headers)
if(NOT wasi_webgpu_headers_POPULATED)
    FetchContent_Populate(wasi_webgpu_headers)
endif()

# The wasi-webgpu-headers repository contains webgpu/webgpu.h
# Add its root directory to the include path
set(WASI_WEBGPU_HEADERS_INCLUDE_DIR ${wasi_webgpu_headers_SOURCE_DIR})

# Create an interface library for this header-only project
add_library(dawn_webgpu_cpp INTERFACE)

target_include_directories(dawn_webgpu_cpp INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${WASI_WEBGPU_HEADERS_INCLUDE_DIR}>
    $<INSTALL_INTERFACE:include>
)

# Export the library for use by other projects
set_target_properties(dawn_webgpu_cpp PROPERTIES
    INTERFACE_CXX_STANDARD 17
    INTERFACE_CXX_STANDARD_REQUIRED ON
)

# Installation support (optional)
install(TARGETS dawn_webgpu_cpp
    EXPORT dawn_webgpu_cppTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(FILES
    webgpu_cpp.h
    webgpu_cpp_chained_struct.h
    webgpu_enum_class_bitmasks.h
    DESTINATION include
)

install(EXPORT dawn_webgpu_cppTargets
    FILE dawn_webgpu_cppTargets.cmake
    NAMESPACE dawn_webgpu_cpp::
    DESTINATION lib/cmake/dawn_webgpu_cpp
)

