cmake_minimum_required(VERSION 3.16)
project(RubiksCube LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(RUBIKS_OUTPUT_DIR "${CMAKE_BINARY_DIR}" CACHE PATH "Directory for RubiksCube build output")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${RUBIKS_OUTPUT_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${RUBIKS_OUTPUT_DIR}")

find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)
find_package(GLEW REQUIRED)

find_package(glm CONFIG REQUIRED)

add_executable(RubiksCube
    src/CubeFinal.cpp
    src/CubeLogic.cpp
    src/CubeRenderer.cpp
    src/GameInterface.cpp
    src/InputSystem.cpp
    src/KeyboardObserver.cpp
    src/RubiksCube.cpp
    src/ShaderUtil.cpp
)

set_target_properties(RubiksCube PROPERTIES
    OUTPUT_NAME "RubiksCube"
)

if(APPLE)
    target_compile_options(RubiksCube PRIVATE -Wno-unknown-pragmas)
endif()

target_include_directories(RubiksCube PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_libraries(RubiksCube PRIVATE
    OpenGL::GL
    glfw
    GLEW::GLEW
    glm::glm
)

if(WIN32)
    target_link_libraries(RubiksCube PRIVATE winmm)
endif()

add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/assets/FColor.glsl
            $<TARGET_FILE_DIR:RubiksCube>/FColor.glsl
)
add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/assets/VColor.glsl
            $<TARGET_FILE_DIR:RubiksCube>/VColor.glsl
)
add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/assets/Click.wav
            $<TARGET_FILE_DIR:RubiksCube>/Click.wav
)
