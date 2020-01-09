# Examples

## Example 1

    cmake_minimum_required(VERSION 3.5)
    
    project(appname VERSION 1.0.0 LANGUAGES CXX)
    
    set(SOURCES
        src/srcfile1.cpp
        src/srcfile2.cpp
    )
    
    add_executable(appname ${SOURCES})
    
    if(UNIX)
        add_definitions(-DLINUX)
    endif()
    
    target_include_directories(appname
        PRIVATE 
            ${PROJECT_SOURCE_DIR}/src/include
            $ENV{FOO_INCLUDE_PATH}
    )

    set(Boost_USE_STATIC_LIBS ON)
    find_package(Boost 1.61.0 REQUIRED thread)  # for windows `MSVC_TOOLSET_VERSION' is used to match boost lib's name
    
    list(APPEND CMAKE_LIBRARY_PATH $ENV{FOO_LIB_PATH})
    find_library(FOO_LIB foo)
    find_library(RT_LIB rt) # 'Boost_USE_STATIC_LIBS ON' need this
    
    # debug
    # include(CMakePrintHelpers)
    # cmake_print_variables(FOO_LIB CMAKE_LIBRARY_PATH)
    
    target_link_libraries(appname
        PRIVATE
            Boost::thread
            ${FOO_LIB}
            ${RT_LIB}
    )

## Example 2

use visual studio 2019 build v100

    {
        "configurations": [{
            "buildCommandArgs": "-v:minimal",
            "buildRoot": "${projectDir}\\build-win32\\${name}",
            "cmakeCommandArgs": "--debug-trycompile -T v100",
            "configurationType": "RelWithDebInfo",
            "ctestCommandArgs": "",
            "generator": "Visual Studio 16 2019",
            "inheritEnvironments": ["msvc_x86"],
            "installRoot": "${projectDir}\\out\\install\\${name}",
            "name": "x86-Release",
            "variables": []
        }]
    }
