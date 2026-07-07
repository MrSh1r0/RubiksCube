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

find_package(glm CONFIG QUIET)
if(TARGET glm::glm)
    set(GLM_INCLUDE_DIRS "")
    set(GLM_TARGET glm::glm)
else()
    find_path(GLM_INCLUDE_DIR glm/glm.hpp
        HINTS
            /opt/homebrew/opt/glm/include
            /opt/homebrew/include
            /usr/local/include
            $ENV{VCPKG_ROOT}/installed/x64-windows/include
            $ENV{VCPKG_ROOT}/installed/x64-windows-static/include
            $ENV{VCPKG_ROOT}/installed/x86-windows/include
            $ENV{VCPKG_ROOT}/installed/x64-windows-md/include
            $ENV{VCPKG_ROOT}/installed/x64-windows-static-md/include
    )

    if(NOT GLM_INCLUDE_DIR)
        message(FATAL_ERROR "GLM headers not found. Install them with Homebrew (brew install glm) or vcpkg.")
    endif()

    set(GLM_INCLUDE_DIRS ${GLM_INCLUDE_DIR})
    set(GLM_TARGET "")
endif()

add_executable(RubiksCube
    RubiksCube/CubeFinal.cpp
    RubiksCube/CubeLogic.cpp
    RubiksCube/CubeRenderer.cpp
    RubiksCube/GameInterface.cpp
    RubiksCube/InputSystem.cpp
    RubiksCube/KeyboardObserver.cpp
    RubiksCube/RubiksCube.cpp
    RubiksCube/ShaderUtil.cpp
)

set_target_properties(RubiksCube PROPERTIES
    OUTPUT_NAME "RubiksCube"
)

if(CMAKE_HOST_SYSTEM_NAME STREQUAL "Darwin")
    target_compile_options(RubiksCube PRIVATE -Wno-unknown-pragmas)
endif()

target_include_directories(RubiksCube PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/RubiksCube
    ${GLM_INCLUDE_DIRS}
)

target_link_libraries(RubiksCube PRIVATE
    OpenGL::GL
    glfw
    GLEW::GLEW
)

if(GLM_TARGET)
    target_link_libraries(RubiksCube PRIVATE ${GLM_TARGET})
endif()

add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/RubiksCube/FColor.glsl
            $<TARGET_FILE_DIR:RubiksCube>/FColor.glsl
)
add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/RubiksCube/VColor.glsl
            $<TARGET_FILE_DIR:RubiksCube>/VColor.glsl
)
add_custom_command(TARGET RubiksCube POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/RubiksCube/Click.wav
            $<TARGET_FILE_DIR:RubiksCube>/Click.wav
)
