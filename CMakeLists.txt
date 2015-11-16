cmake_minimum_required(VERSION 2.8.3)
project(rviz_plugin_covariance)

add_definitions(-DQT_NO_KEYWORDS)

find_package(catkin REQUIRED 
	COMPONENTS rviz roscpp geometry_msgs nav_msgs cmake_modules)
find_package(Eigen3 REQUIRED)
find_package(Qt4 COMPONENTS QtCore QtGui REQUIRED)

catkin_package(
	DEPENDS eigen
	INCLUDE_DIRS src ${EIGEN_INCLUDE_DIR}
	CATKIN_DEPENDS rviz roscpp geometry_msgs nav_msgs
)

qt4_wrap_cpp(MOC_FILES
  src/covariance_display.h
)

set(SOURCE_FILES
  src/covariance_display.cpp
  src/covariance_visual.cpp
  ${MOC_FILES}
)

include(${QT_USE_FILE})

include_directories(${EIGEN_INCLUDE_DIR})
include_directories(${catkin_INCLUDE_DIRS})

add_library(${PROJECT_NAME} ${SOURCE_FILES})
add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES} ${catkin_LIBRARIES})


## Install rules

install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(FILES
  plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(DIRECTORY icons/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/icons)

install(PROGRAMS scripts/send_test_msgs.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
