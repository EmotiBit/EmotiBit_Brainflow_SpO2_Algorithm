cmake_minimum_required(VERSION 4.1)
set(CMAKE_POLICY_VERSION_MINIMUM "4.1")
set(ARDUINO_INSTALL_PATH "C:/Users/jxie7/AppData/Local/Arduino15")
set(ARDUINO_BOARD "Adafruit ESP32 Feather [esp32.featheresp32]")

# Include modules
include("${CMAKE_CURRENT_SOURCE_DIR}/Arduino-toolchain.cmake")

project(BrainflowSpO2Algorithm VERSION 4.1 LANGUAGES C CXX) # Project name becomes name of the Python module

# See pybind11Config.cmake, instructions for pybind11_DIR
# According to CMake convention, find_package searches in path set in the <package_name>_DIR 
set(pybind11_DIR "${CMAKE_CURRENT_SOURCE_DIR}/venv/Lib/site-packages/pybind11/share/cmake/pybind11")
find_package(Python COMPONENTS Interpreter Development REQUIRED)
find_package(pybind11 CONFIG REQUIRED)

# CMake function is a convenience helper provided by the Pybind11 project
pybind11_add_module(BrainflowSpO2Algorithm "${CMAKE_CURRENT_SOURCE_DIR}/bindings.cpp")

target_link_arduino_libraries(BrainflowSpO2Algorithm AUTO_PUBLIC)

# # gather all source files
# file(GLOB SRC
#   "${CMAKE_CURRENT_SOURCE_DIR}/../src/*.cpp"
#   "C:/Users/jxie7/Documents/Arduino/libraries/EmotiBit_DSPFilters/src/*.cpp"
# )

# # Add local source files
# add_library(lib STATIC 
#   ${SRC}
# )
# # Add header files to be included
# target_include_directories(lib
#   PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/../src"
#   PUBLIC "C:/Users/jxie7/Documents/Arduino/libraries/EmotiBit_DSPFilters/src/"
# )

# # CMake function is a convenience helper provided by the Pybind11 project
# pybind11_add_module(BrainflowSpO2Algorithm "${CMAKE_CURRENT_SOURCE_DIR}/bindings.cpp")

# target_link_arduino_libraries(BrainflowSpO2Algorithm PRIVATE core)
# target_link_libraries(BrainflowSpO2Algorithm PRIVATE lib)