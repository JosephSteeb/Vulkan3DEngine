cmake_minimum_required(VERSION 3.12)
project(MyProject VERSION 1.0)

# Set C++20 standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Set the generator to Ninja if it's not already set
if (NOT CMAKE_GENERATOR)
    set(CMAKE_GENERATOR "Ninja" CACHE INTERNAL "CMake generator" FORCE)
endif()

# Add your source files
add_executable(MyApp src/main.cpp src/lve_app_base.cpp src/lve_window.cpp src/lve_pipeline.cpp src/lve_app_base.hpp src/lve_window.hpp src/lve_pipeline.hpp)

# Specify the path to the 'lib' folder in your project
set(LIBS_DIR "${CMAKE_SOURCE_DIR}/lib")

find_package(Vulkan REQUIRED)

if (Vulkan_FOUND)
    message(STATUS "Found Vulkan: ${Vulkan_INCLUDE_DIRS}")
    target_include_directories(MyApp PRIVATE ${Vulkan_INCLUDE_DIRS})
    target_link_libraries(MyApp PRIVATE ${Vulkan_LIBRARIES})
else()
    message(FATAL_ERROR "Vulkan not found on your system.")
endif()

# Add the subdirectories containing the libraries' CMakeLists.txt files
add_subdirectory("${LIBS_DIR}/glfw-3.3.8")
add_subdirectory("${LIBS_DIR}/glm")

# Link GLFW to your executable (assuming 'glfw' is the target name in glfw-3.3.8/CMakeLists.txt)
target_link_libraries(MyApp PRIVATE glfw)

# Include GLM headers from the 'glm' subdirectory
target_include_directories(MyApp PRIVATE "${LIBS_DIR}/glm")
