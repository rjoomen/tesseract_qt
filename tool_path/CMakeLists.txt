find_package(tesseract_common REQUIRED)
find_package(Qt5 COMPONENTS Core Gui Widgets REQUIRED)

# Load variable for clang tidy args, compiler options and cxx version
tesseract_variables()

qt5_wrap_cpp(TOOL_PATH_MODEL_headers_MOC include/tesseract_qt/tool_path/models/tool_path_selection_model.h
             include/tesseract_qt/tool_path/models/tool_path_model.h)

add_library(
  ${PROJECT_NAME}_tool_path_models SHARED
  ${TOOL_PATH_MODEL_headers_MOC}
  src/models/tool_path_selection_model.cpp
  src/models/tool_path_model.cpp
  src/models/tool_path_utils.cpp)
target_link_libraries(
  ${PROJECT_NAME}_tool_path_models
  PUBLIC tesseract::tesseract_common
         Qt5::Core
         Qt5::Gui
         ${PROJECT_NAME}_common)
target_include_directories(
  ${PROJECT_NAME}_tool_path_models
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>" "$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/tool_path>"
         "$<INSTALL_INTERFACE:include>")

qt5_wrap_cpp(
  TOOL_PATH_WIDGET_headers_MOC
  include/tesseract_qt/tool_path/widgets/tool_path_widget.h
  include/tesseract_qt/tool_path/widgets/tool_path_tool_bar.h
  include/tesseract_qt/tool_path/widgets/tool_path_file_dialog.h)

qt5_wrap_ui(TOOL_PATH_WIDGET_headers_UI include/tesseract_qt/tool_path/widgets/tool_path_file_dialog.ui)

add_library(
  ${PROJECT_NAME}_tool_path_widgets SHARED
  ${TOOL_PATH_WIDGET_headers_MOC}
  ${TOOL_PATH_WIDGET_headers_UI}
  src/widgets/tool_path_file_dialog.cpp
  src/widgets/tool_path_tool_bar.cpp
  src/widgets/tool_path_widget.cpp)

target_link_libraries(
  ${PROJECT_NAME}_tool_path_widgets
  PUBLIC tesseract::tesseract_common
         Qt5::Core
         Qt5::Gui
         Qt5::Widgets
         ${PROJECT_NAME}_tool_path_models)
target_include_directories(
  ${PROJECT_NAME}_tool_path_widgets
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>" "$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/tool_path>"
         "$<INSTALL_INTERFACE:include>")

add_subdirectory(demo)

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME} DESTINATION include)

# Install
list(
  APPEND
  PROJECT_TARGETS
  ${PROJECT_NAME}_tool_path_models
  ${PROJECT_NAME}_tool_path_widgets)
set(PROJECT_TARGETS ${PROJECT_TARGETS} PARENT_SCOPE)
install_targets(TARGETS ${PROJECT_NAME}_tool_path_models ${PROJECT_NAME}_tool_path_widgets)
