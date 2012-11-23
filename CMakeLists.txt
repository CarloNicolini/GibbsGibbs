project(GibbsGibbs)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6.0 FATAL_ERROR)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")
set(PROJECT_BINARY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/build/")
set(LIBRARY_OUTPUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build/" CACHE PATH "Single directory for all libraries")
set(EXECUTABLE_OUTPUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build/" CACHE PATH "Single directory for all executables")

####################### FIND PACKAGES AND LIBRARIES ####################
find_package(StandardMathLibrary)

set(BOOST_LIBS thread date_time system program_options filesystem regex serialization iostreams)

set(Boost_USE_STATIC_LIBS        ON)
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME    OFF)
 
link_directories ( ${Boost_LIBRARY_DIRS} )
include_directories ( ${Boost_INCLUDE_DIRS} )
find_package(Boost COMPONENTS ${BOOST_LIBS} REQUIRED)
include_directories(${Boost_INCLUDE_DIR})
find_package(Threads REQUIRED)
####################### END BOOST LIBRARIES INCLUDE AND LINKING ################


########################## IF MICROSOFT COMPILER ###############################
# Push more optimization options with Microsoft Compiler
if(MSVC)
# This set the following defines when compiling on windows
# _WIN32, _NOMINMAX, disable some stupid warnings and enable multiprocessor compilation (/MP)
add_definitions("-D_WIN32 -DNOMINMAX /EHsc /wd4127 /wd4129 /wd4505 /wd4714 /wd4800 /wd4181 /wd4244 /wd4996 /MP -D_WIN32_WINNT=0x0501")
# This push some other optimizations in order: Full optimization (Ox), omit frame pointer (Oy), Enable FiberSafe optimizations(GT),
# Whole Program Optimization (/GL), OpenMP support (/openmp), NONdynamic runtime libraries (MT)
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /arch:SSE2 /Ox /Oy /GT /GL")
# Specify to use multithreaded debug (non dynamic)
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
endif(MSVC)
####################### END IF MICROSOFT COMPILER #############################

# Activates OpenMP on architectures which support it
include(FindOpenMP)
if(OPENMP_FOUND)
	message(STATUS "Adding OpenMP flags to MSVC: ${OpenMP_CXX_FLAGS} and ${OpenMP_EXE_LINKER_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR} ${PROJECT_BINARY_DIR} )

add_subdirectory(src)
