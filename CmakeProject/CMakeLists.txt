cmake_minimum_required(VERSION 3.10)
project(anms CXX)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(OpenCV REQUIRED)

add_library(anms INTERFACE)
target_include_directories(
  anms INTERFACE $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
                 $<INSTALL_INTERFACE:include> ${OpenCV_INCLUDE_DIRS})
target_link_libraries(anms INTERFACE ${OpenCV_LIBS})

add_executable(anms_example src/main.cpp)
target_link_libraries(anms_example anms)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${PROJECT_BINARY_DIR}/anmsConfigVersion.cmake"
  VERSION 0.1
  COMPATIBILITY AnyNewerVersion)

install(
  TARGETS anms
  EXPORT anmsTargets
  LIBRARY DESTINATION lib COMPONENT Runtime
  ARCHIVE DESTINATION lib COMPONENT Development
  RUNTIME DESTINATION bin COMPONENT Runtime
  PUBLIC_HEADER DESTINATION include COMPONENT Development
  BUNDLE DESTINATION bin COMPONENT Runtime)

configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/anmsConfig.cmake.in"
  "${PROJECT_BINARY_DIR}/anmsConfig.cmake" INSTALL_DESTINATION lib/cmake/anms)

install(EXPORT anmsTargets DESTINATION lib/cmake/anms)
install(FILES "${PROJECT_BINARY_DIR}/anmsConfigVersion.cmake"
              "${PROJECT_BINARY_DIR}/anmsConfig.cmake"
        DESTINATION lib/cmake/anms)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)
