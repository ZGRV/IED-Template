cmake_minimum_required(VERSION 3.10)

# Enable cross-compilation against the aarch64 sysroot toolchain.
# Pass -DCROSS_COMPILE=ON at configure time to build for the target;
# leave OFF (default) to build natively on the host.
option(CROSS_COMPILE "Enable cross-compilation with the aarch64-buildroot sysroot toolchain" OFF)

if(CROSS_COMPILE)
    include("${CMAKE_CURRENT_LIST_DIR}/cmake/toolchain.cmake")
endif()

project(IED C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

include(third_party/CMakeLists.txt)

# Add executable
add_executable(ied_exe src/main.c)

# Add include directories
target_include_directories(ied_exe PRIVATE include ${LIBIEC_INCLUDES})

# Set compiler options
target_compile_options(ied_exe PRIVATE -Wall -Wextra -pedantic)
target_link_libraries(ied_exe PRIVATE iec61850)


