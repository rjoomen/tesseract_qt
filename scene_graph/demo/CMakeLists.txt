find_package(tesseract_urdf REQUIRED)

add_executable(${PROJECT_NAME}_scene_graph_demo scene_graph_demo.cpp)
target_link_libraries(${PROJECT_NAME}_scene_graph_demo
                      PRIVATE tesseract::tesseract_urdf ${PROJECT_NAME}_scene_graph_widgets ${PROJECT_NAME}_common)

install_targets(TARGETS ${PROJECT_NAME}_scene_graph_demo)
