find_package(tesseract_urdf REQUIRED)

add_executable(${PROJECT_NAME}_kinematic_groups_demo kinematic_group_demo.cpp)
target_link_libraries(${PROJECT_NAME}_kinematic_groups_demo PRIVATE ${PROJECT_NAME}_kinematic_groups_widgets)

add_executable(${PROJECT_NAME}_groups_joint_states_demo groups_joint_states_demo.cpp)
target_link_libraries(${PROJECT_NAME}_groups_joint_states_demo PRIVATE ${PROJECT_NAME}_kinematic_groups_widgets
                                                                       tesseract::tesseract_urdf)

install_targets(TARGETS ${PROJECT_NAME}_kinematic_groups_demo ${PROJECT_NAME}_groups_joint_states_demo)
