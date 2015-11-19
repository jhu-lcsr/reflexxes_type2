cmake_minimum_required(VERSION 2.8)

project(reflexxes_type2)

include(ExternalProject)

set(reflexxes_type2_VERSION 1.2.6)
# http://www.reflexxes.ws/software/typeiirml/v1.2.6/files/ReflexxesTypeII.zip
# https://github.com/jhu-lcsr/reflexxes_type2/blob/upstream-1.2.6/ReflexxesTypeII.zip?raw=true

set(RML_HEADER_DIR "${CMAKE_BINARY_DIR}/reflexxes-prefix/src/reflexxes/include/")
set(RML_RELEASE_LIBRARY_DIR "${CMAKE_BINARY_DIR}/reflexxes-prefix/src/reflexxes/Linux/x64/release/lib/shared/")
set(RML_DEBUG_LIBRARY_DIR "${CMAKE_BINARY_DIR}/reflexxes-prefix/src/reflexxes/Linux/x64/debug/lib/shared/")

ExternalProject_Add(
  reflexxes
  URL "https://github.com/jhu-lcsr/reflexxes_type2/blob/upstream-${reflexxes_type2_VERSION}/ReflexxesTypeII.zip?raw=true"
  DOWNLOAD_NAME ReflexxesTypeII.zip
  CONFIGURE_COMMAND ""
  BINARY_DIR reflexxes-prefix/src/reflexxes/Linux
  BUILD_COMMAND make clean64 all64
  INSTALL_COMMAND ls "${RML_HEADER_DIR}"
  )

install(
  DIRECTORY ${RML_HEADER_DIR}
  DESTINATION include
  FILES_MATCHING PATTERN "*.h"
  )

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  install(DIRECTORY ${RML_DEBUG_LIBRARY_DIR}
    DESTINATION lib
    FILES_MATCHING PATTERN "*${CMAKE_SHARED_LIBRARY_SUFFIX}"
    )
else()
  install(DIRECTORY ${RML_RELEASE_LIBRARY_DIR}
    DESTINATION lib
    FILES_MATCHING PATTERN "*${CMAKE_SHARED_LIBRARY_SUFFIX}"
    )
endif()

configure_file(reflexxes_type2-config.cmake.in
  ${PROJECT_BINARY_DIR}/reflexxes_type2-config.cmake @ONLY)
configure_file(reflexxes_type2-config-version.cmake.in
  ${PROJECT_BINARY_DIR}/reflexxes_type2-config-version.cmake @ONLY)

install(FILES ${PROJECT_BINARY_DIR}/reflexxes_type2-config.cmake DESTINATION share/reflexxes_type2/cmake)
install(FILES ${PROJECT_BINARY_DIR}/reflexxes_type2-config-version.cmake DESTINATION share/reflexxes_type2/cmake)
install(FILES package.xml DESTINATION share/reflexxes_type2)

