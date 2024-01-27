cmake_minimum_required(VERSION 3.10)
project(gSDK)

find_package(ament_cmake REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(Threads REQUIRED)

#################
# Setup targets #
#################

include_directories(include)

add_library(gSDK SHARED
  src/gimbal_interface.cpp
  src/gimbal_protocol_v1.cpp
  src/gimbal_protocol_v2.cpp
  src/gimbal_protocol.cpp
  src/serial_port.cpp)

ament_target_dependencies(gSDK Threads)

target_include_directories(gSDK PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/${PROJECT_NAME}>
	$<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)

############
# Install ##
############

# Export information to downstream packages
ament_export_dependencies(ament_cmake Threads)

ament_export_targets(export_gSDK HAS_LIBRARY_TARGET)

ament_export_include_directories(include)
ament_export_libraries(gSDK)

# Install header files
install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION include/${PROJECT_NAME})

install(TARGETS gSDK
	EXPORT export_gSDK
	ARCHIVE DESTINATION lib
	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
	INCLUDES DESTINATION include
)

ament_package()