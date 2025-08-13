cmake_minimum_required(VERSION 3.14)

project(BrainflowSpO2Algorithm VERSION 4.1 LANGUAGES C CXX) # Project name becomes name of the Python module

# GoogleTest requires at least C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Set Arduino directory variable
IF (WIN32)
  set(ARDUINO_DIRECTORY "$ENV{USERPROFILE}/Documents/Arduino")
ELSE()
  set(ARDUINO_DIRECTORY "$ENV{HOME}/Arduino")
ENDIF()

# ----------------------------------------------------------------------------------------------------------
# Arduino Mock Library Setup
# ----------------------------------------------------------------------------------------------------------

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()

# Setup mock Arduino library to simulate Arduino environment
add_library(ArduinoMock
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core/Arduino.cpp"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core/ArduinoMock.cpp"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core/HardwareSerial0.cpp"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core/Print.cpp"
)
target_include_directories(ArduinoMock PUBLIC
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Core-Libraries"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Libraries/Adafruit_GFX"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Libraries/Adafruit_SSD1306"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/mock/Libraries/Encoder"
)
target_link_libraries(ArduinoMock PUBLIC GTest::gmock_main GTest::gmock GTest::gtest_main GTest::gtest) 

# ----------------------------------------------------------------------------------------------------------
# Algorithm/pybind11 Setup
# ----------------------------------------------------------------------------------------------------------

# See pybind11Config.cmake, instructions for pybind11_DIR
# According to CMake convention, find_package searches in path set in the <package_name>_DIR 
set(pybind11_DIR "${CMAKE_CURRENT_SOURCE_DIR}/venv/Lib/site-packages/pybind11/share/cmake/pybind11")
find_package(Python COMPONENTS Interpreter Development REQUIRED)
find_package(pybind11 CONFIG REQUIRED)

# Gather all source files
file(GLOB SRC
  "${CMAKE_CURRENT_SOURCE_DIR}/../src/*.cpp"
  # Include other .cpp files as needed
)

# Add local source files
add_library(lib STATIC 
  ${SRC}
)

# Add header files to be included
target_include_directories(lib PUBLIC 
  "${CMAKE_CURRENT_SOURCE_DIR}/../src"
  "${ARDUINO_DIRECTORY}/libraries/EmotiBit_ArduinoFilters/src"
)
target_link_libraries(lib PUBLIC ArduinoMock)

# CMake function is a convenience helper provided by the Pybind11 project
pybind11_add_module(BrainflowSpO2Algorithm "${CMAKE_CURRENT_SOURCE_DIR}/bindings.cpp")

target_link_libraries(BrainflowSpO2Algorithm PUBLIC
  lib
)
