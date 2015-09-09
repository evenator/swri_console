cmake_minimum_required(VERSION 2.8.3)
project(swri_console)

find_package(catkin REQUIRED COMPONENTS roscpp rosgraph_msgs)
find_package(Qt4 REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  DEPENDS libqt4
  CATKIN_DEPENDS roscpp rosgraph_msgs
)

include(${QT_USE_FILE})
include_directories(include 
  ${catkin_INCLUDE_DIRS} 
  ${QT_INCLUDE_DIR} )
add_definitions(${QT_DEFINITIONS})

set(QT_USE_QTCORE TRUE)
set(QT_USE_QTGUI TRUE)

# Build mapviz node
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(src)
file (GLOB UI_FILES
  ui/console_window.ui)
file (GLOB HEADER_FILES
  include/swri_console/console_master.h
  include/swri_console/console_window.h
  include/swri_console/log_database.h
  include/swri_console/node_list_model.h
  include/swri_console/log_database_proxy_model.h)
file (GLOB SRC_FILES
  src/console_master.cpp
  src/console_window.cpp
  src/log_database.cpp
  src/node_list_model.cpp
  src/log_database_proxy_model.cpp)
QT4_ADD_RESOURCES(RCC_SRCS resources/images.qrc)
QT4_WRAP_UI(SRC_FILES ${UI_FILES})
QT4_WRAP_CPP(SRC_FILES ${HEADER_FILES})

add_executable(swri_console ${SRC_FILES} ${RCC_SRCS} src/main.cpp)
target_link_libraries(${PROJECT_NAME} 
  ${QT_LIBRARIES}
  ${catkin_LIBRARIES})

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)