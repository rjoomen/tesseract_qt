find_package(tesseract_common REQUIRED)
find_package(Qt5 COMPONENTS Core Gui Widgets REQUIRED)

# Load variable for clang tidy args, compiler options and cxx version
tesseract_variables()

qt5_wrap_cpp(WORKBENCH_WIDGET_headers_MOC include/tesseract_qt/workbench/workbench_widget.h)
qt5_wrap_ui(WORKBENCH_headers_UI include/tesseract_qt/workbench/workbench_widget.ui)

add_library(${PROJECT_NAME}_workbench_widgets SHARED ${WORKBENCH_WIDGET_headers_MOC} ${WORKBENCH_headers_UI}
                                                     src/workbench_widget.cpp)
target_link_libraries(
  ${PROJECT_NAME}_workbench_widgets
  PUBLIC tesseract::tesseract_common
         Qt5::Core
         Qt5::Gui
         Qt5::Widgets
         ${PROJECT_NAME}_environment_widgets
         ${PROJECT_NAME}_joint_trajectory_widgets
         ${PROJECT_NAME}_manipulation_widgets
         ${PROJECT_NAME}_planning_widgets)
target_include_directories(
  ${PROJECT_NAME}_workbench_widgets
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>" "$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/workbench>"
         "$<INSTALL_INTERFACE:include>")

add_subdirectory(demo)

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME} DESTINATION include)

# Install
list(APPEND PROJECT_TARGETS ${PROJECT_NAME}_workbench_widgets)
set(PROJECT_TARGETS ${PROJECT_TARGETS} PARENT_SCOPE)
install_targets(TARGETS ${PROJECT_NAME}_workbench_widgets)
