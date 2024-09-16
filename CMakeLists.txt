cmake_minimum_required(VERSION 3.12)

# Include build functions from Pico SDK
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

set(PICO_BOARD pico_w)

# Set name of project as PROJECT_NAME and C/C standards
project(blinky C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Create a pico-sdk subdirectory in the project for the libraries
pico_sdk_init()

add_compile_options(-Wall
					-Wno-format
					-Wno-unused-function
					-Wno-maybe-uninitialized
)

# Create a variable for all source files in the src directory
file(GLOB_RECURSE SOURCES src/*.c)

add_executable(${PROJECT_NAME} ${SOURCES})

# Create map/bin/hex/uf2 files for flashing to the Pico
pico_add_extra_outputs(${PROJECT_NAME})

# Include header directory
target_include_directories(${PROJECT_NAME} PRIVATE inc)

# Link to the Pico SDK standard library
target_link_libraries(${PROJECT_NAME}
	pico_stdlib
)

# Enable USB output and disable UART
pico_enable_stdio_usb(${PROJECT_NAME} 0)
pico_enable_stdio_uart(${PROJECT_NAME} 1)

