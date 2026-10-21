cmake_minimum_required(VERSION 2.8.3)
project(lkh_tsp_solver)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_CXX_FLAGS "-O3 -Wall")

find_package(catkin REQUIRED COMPONENTS
  roscpp
)

catkin_package(
 INCLUDE_DIRS include
 LIBRARIES lkh_tsp_solver
 CATKIN_DEPENDS roscpp
#  DEPENDS system_lib
)

include_directories( 
    SYSTEM 
    include 
)

file( GLOB SRCS src/*.c )

# message("${SRCS}")

add_library( lkh_tsp_solver 
  ${SRCS}
  src/lkh_interface.cpp
)
target_link_libraries(lkh_tsp_solver -lm)

add_executable( lkh_tsp 
  ${SRCS}
)
target_link_libraries(lkh_tsp ${catkin_LIBRARIES} -lm)

#Codex: This bundled C solver uses tentative globals; preserve its pre-GCC-10 common-symbol semantics.
target_compile_options(lkh_tsp_solver PRIVATE $<$<COMPILE_LANGUAGE:C>:-fcommon>)
target_compile_options(lkh_tsp PRIVATE $<$<COMPILE_LANGUAGE:C>:-fcommon>)
