cmake_minimum_required (VERSION 3.8)

#----Make changes here

#This is your project name. And also the filename of the resulting plugin.
project (template-plugin)

#This setting enables the use of the engine string type instead of converting to std::string.
#Enabling this results in better performance when handling strings to SQF commands.
option(USE_ENGINE_TYPES "USE_ENGINE_TYPES" OFF)

#Which version of Intercept to build against, branch/commit/tag
set(INTERCEPT_VERSION "dfa2ce681154337a1640ea7fa2d00576fb44349b")

#----Don't change anything below this line

option(USE_64BIT_BUILD "USE_64BIT_BUILD" OFF)
set(INTERCEPT_LINK_TYPE "static")

if("${CMAKE_SIZEOF_VOID_P}" STREQUAL "8")
	set( USE_64BIT_BUILD ON)
endif()

message("GENERATOR USED: '${CMAKE_GENERATOR}'")
message("COMPILER USED: '${CMAKE_CXX_COMPILER_ID}'")

set(CMAKE_CL_64 ${USE_64BIT_BUILD})

if(USE_64BIT_BUILD)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/build/win64/")
else()
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/build/win32/")
endif()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_INCLUDE_CURRENT_DIR ON) 
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)

add_subdirectory(src)

include(ExternalProject)

# Download Intercept library to vendor folder
ExternalProject_Add(
    intercept
    PREFIX ${CMAKE_CURRENT_LIST_DIR}/vendor/intercept
    URL https://github.com/intercept/intercept/archive/${INTERCEPT_VERSION}.tar.gz
    BUILD_COMMAND ""
    CONFIGURE_COMMAND ""
    INSTALL_COMMAND ""
)
