find_package(tesseract_common REQUIRED)
find_package(tesseract_visualization REQUIRED)
find_package(tesseract_command_language REQUIRED)
find_package(Qt5 COMPONENTS Core Gui Widgets REQUIRED)

# Load variable for clang tidy args, compiler options and cxx version
tesseract_variables()

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

set(CMAKE_AUTOUIC_SEARCH_PATHS include/tesseract_qt/joint_trajectory/widgets)

set(TRAJ_MODEL_headers_MOC include/tesseract_qt/joint_trajectory/models/joint_trajectory_model.h)

add_library(
  ${PROJECT_NAME}_joint_trajectory_models SHARED
  ${TRAJ_MODEL_headers_MOC}
  src/models/joint_trajectory_info_item.cpp
  src/models/joint_trajectory_set_item.cpp
  src/models/joint_trajectory_state_item.cpp
  src/models/joint_trajectory_model.cpp
  src/models/joint_trajectory_utils.cpp)
target_link_libraries(
  ${PROJECT_NAME}_joint_trajectory_models
  PUBLIC tesseract::tesseract_common
         Qt5::Core
         Qt5::Gui
         ${PROJECT_NAME}_common)
target_include_directories(
  ${PROJECT_NAME}_joint_trajectory_models PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
                                                 "$<INSTALL_INTERFACE:include>")

set(TRAJ_WIDGET_headers_MOC
    include/tesseract_qt/joint_trajectory/widgets/joint_trajectory_widget.h
    include/tesseract_qt/joint_trajectory/widgets/joint_trajectory_tool_bar.h
    include/tesseract_qt/joint_trajectory/widgets/joint_trajectory_plot_dialog.h)

add_library(
  ${PROJECT_NAME}_joint_trajectory_widgets SHARED
  ${TRAJ_WIDGET_headers_MOC}
  src/widgets/joint_trajectory_tool_bar.cpp
  src/widgets/joint_trajectory_widget.cpp
  src/widgets/joint_trajectory_plot_dialog.cpp)

target_link_libraries(
  ${PROJECT_NAME}_joint_trajectory_widgets
  PUBLIC tesseract::tesseract_common
         tesseract::tesseract_visualization
         tesseract::tesseract_command_language
         Qt5::Core
         Qt5::Gui
         Qt5::Widgets
         ${PROJECT_NAME}_plot
         ${PROJECT_NAME}_common
         ${PROJECT_NAME}_joint_trajectory_models)
target_include_directories(
  ${PROJECT_NAME}_joint_trajectory_widgets
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
         "$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/joint_trajectory>" "$<INSTALL_INTERFACE:include>")

add_subdirectory(demo)

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME} DESTINATION include)

# Install
list(
  APPEND
  PROJECT_TARGETS
  ${PROJECT_NAME}_joint_trajectory_models
  ${PROJECT_NAME}_joint_trajectory_widgets)
set(PROJECT_TARGETS ${PROJECT_TARGETS} PARENT_SCOPE)
install_targets(TARGETS ${PROJECT_NAME}_joint_trajectory_models ${PROJECT_NAME}_joint_trajectory_widgets)
