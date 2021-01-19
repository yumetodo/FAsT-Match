cmake_minimum_required(VERSION 3.10.0)
enable_language(CXX)
set(CMAKE_CXX_STANDARD 20) # C++20...
set(CMAKE_CXX_STANDARD_REQUIRED ON) #...is required...
set(CMAKE_CXX_EXTENSIONS OFF) #...without compiler extensions like gnu++11
find_package(OpenCV REQUIRED)
find_package(TBB REQUIRED tbb)

## Set our project name
project(FAsTMatch)

set(FAsTMatch_src
  FAsT-Match/FAsTMatch.cpp
  FAsT-Match/FAsTMatch.h
  FAsT-Match/MatchConfig.cpp
  FAsT-Match/MatchConfig.h
  FAsT-Match/MatchNet.cpp
  FAsT-Match/MatchNet.h
  FAsT-Match/main.cpp
)
if(MSVC)
  # Force to always compile with W4
  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
  endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
  # Update if necessary
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-long-long -pedantic")
endif()
## Define the executable
add_executable(FAsTMatch ${FAsTMatch_src})
target_include_directories(FAsTMatch PRIVATE ${OpenCV_INCLUDE_DIRS})
target_link_libraries(FAsTMatch ${OpenCV_LIBS} TBB::tbb)
