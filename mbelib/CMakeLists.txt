project(mbe)
cmake_minimum_required(VERSION 2.6)
 
FILE(GLOB SRCS *.c)
 
include_directories("${PROJECT_SOURCE_DIR}")
 
ADD_LIBRARY(mbe-static STATIC ${SRCS})
ADD_LIBRARY(mbe-shared SHARED ${SRCS})
TARGET_LINK_LIBRARIES(mbe-static m)
TARGET_LINK_LIBRARIES(mbe-shared m)
 
install(TARGETS mbe-static mbe-shared DESTINATION lib)
install (FILES "${PROJECT_SOURCE_DIR}/mbelib.h" DESTINATION include)
 
set_target_properties(mbe-static mbe-shared PROPERTIES OUTPUT_NAME mbe
                      VERSION 1.0 SOVERSION 1 INSTALL_NAME_DIR ${CMAKE_INSTALL_PREFIX}/lib )

# uninstall target
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)