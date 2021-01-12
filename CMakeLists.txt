cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

project(utilslib)

include(GNUInstallDirs)

set(TARGET_NAME utilslib)

# setup Conan (if we are in Conan)
if(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup()
endif()

# provide buildable target
file(GLOB SOURCE_FILES src/*.cpp)
add_library(${TARGET_NAME} ${SOURCE_FILES})
target_include_directories(${TARGET_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
                                                 "$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>")

# provide installable target
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} FILES_MATCHING PATTERN *.h*)
install(TARGETS ${TARGET_NAME} EXPORT ${TARGET_NAME}-config)

# export CMake target
install(EXPORT ${TARGET_NAME}-config DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${TARGET_NAME})
