cmake_minimum_required(VERSION 3.28.0)
project(firstsoloproj VERSION 0.1.0)

# This generates the compile_commands.json automatically in your build folder
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Helps CMake find OpenGL correctly (from your old project)
cmake_policy(SET CMP0072 NEW)

if(EMSCRIPTEN)
    message(STATUS "Building for Web (Emscripten)")
else()
    message(STATUS "Building for Native Desktop")
    find_package(OpenGL REQUIRED)
    find_package(glfw3 REQUIRED) # This finds the config files
    find_package(assimp REQUIRED)
    find_package(Threads REQUIRED)
    
    # Matching your old project's link names
    set(PLATFORM_LIBS 
        glfw              # Linked as 'glfw' in your old project
        OpenGL::GL 
        assimp            # Linked as 'assimp' in your old project
        Threads::Threads
    )
endif()

add_executable(firstsoloproj
    src/main.cpp 
    src/Texture.cpp
    src/tinygltf.cpp
    src/glad.c        
)

target_include_directories(firstsoloproj PUBLIC 
    src 
    src/glad
    src/vendor
    src/vendor/glm
    src/vendor/TinyGLTF
)

if(NOT EMSCRIPTEN)
    target_link_libraries(firstsoloproj PUBLIC ${PLATFORM_LIBS})
endif()

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/src/res DESTINATION ${CMAKE_CURRENT_BINARY_DIR})