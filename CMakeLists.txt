cmake_minimum_required(VERSION 3.16)


## Search for programs in the build host directories
SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

## Libraries and headers in the target directories
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

project(LTOF
        VERSION 1.0.0
        DESCRIPTION "TOF")


## Append the version number to the target name
set(TARGET_NAME "${PROJECT_NAME}_${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")

# Set the target architecture and compiler flags
set(CMAKE_OSX_ARCHITECTURES "armv7-a")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=armv7-a -mtune=cortex-a7 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -O2 -fdata-sections -fno-builtin -fno-common")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=armv7-a -mtune=cortex-a7 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -O2")


set(CMAKE_CXX_STANDARD 11)

add_executable(LTOF src/main.c
        core/VL53L1X_api.c
        core/VL53L1X_api.h
        core/VL53L1X_calibration.c
        core/VL53L1X_calibration.h
        platform/vl53l1_platform.h
        platform/vl53l1_types.h
        platform/vl53l1_linux_platform.c)


# Get the version components
string(TOLOWER "${PROJECT_NAME}_${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}" TARGET_NAME)

# Set C++ standard and linker flags
set_target_properties(LTOF PROPERTIES
        OUTPUT_NAME "${TARGET_NAME}"
        CXX_STANDARD 11  # Use the appropriate C++ standard version
        CXX_STANDARD_REQUIRED YES
        LINK_FLAGS "-static"
)