cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
project(libTorchTrain)

# C++ Standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required packages
find_package(Torch REQUIRED)
find_package(MKL REQUIRED)
find_package(OpenCV REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})

# Executable
add_executable(libTorchTrain main.cpp)

# Link libraries
target_link_libraries(libTorchTrain "${TORCH_LIBRARIES}")
target_link_directories(libTorchTrain PRIVATE "C:/Program Files (x86)/Intel/oneAPI/mkl/latest/lib/intel64")
target_link_libraries(libTorchTrain MKL::MKL)
target_link_libraries(libTorchTrain ${OpenCV_LIBS})

# --- MSVC Runtime Fix (/MD statt /MDd) ---
if (MSVC)
  message(STATUS "Patching MSVC runtime flags to use /MD (not /MDd)")
  foreach(flag_var
          CMAKE_C_FLAGS CMAKE_C_FLAGS_DEBUG CMAKE_C_FLAGS_RELEASE
          CMAKE_C_FLAGS_RELWITHDEBINFO CMAKE_C_FLAGS_MINSIZEREL
          CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
          CMAKE_CXX_FLAGS_RELWITHDEBINFO CMAKE_CXX_FLAGS_MINSIZEREL)
    if(${flag_var} MATCHES "/MDd")
      string(REPLACE "/MDd" "/MD" ${flag_var} "${${flag_var}}")
    endif()
  endforeach()
endif()

# Optionale Ausgabe zur Kontrolle
message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message(STATUS "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")