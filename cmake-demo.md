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

`CMakeSettings.json`

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

## Example 3

`gtest`

    find_package(Threads REQUIRED)

    # Enable ExternalProject CMake module
    include(ExternalProject)

    # Download and install GoogleTest
    ExternalProject_Add(
        gtest
        URL https://github.com/google/googletest/archive/release-1.8.0.zip
        PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gtest

        # cmake arguments
        CMAKE_ARGS -Dgtest_force_shared_crt=ON

        # Disable install step
        INSTALL_COMMAND ""
    )

    # Get GTest source and binary directories from CMake project
    ExternalProject_Get_Property(gtest source_dir binary_dir)


    if(UNIX)
        # Create a libgtest target to be used as a dependency by test programs
        add_library(libgtest IMPORTED STATIC GLOBAL)
        add_dependencies(libgtest gtest)

        # Set libgtest properties
        set_target_properties(libgtest PROPERTIES
            "IMPORTED_LOCATION" "${binary_dir}/googlemock/gtest/libgtest.a"
            "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
        )

        # Create a libgmock target to be used as a dependency by test programs
        add_library(libgmock IMPORTED STATIC GLOBAL)
        add_dependencies(libgmock gtest)

        # Set libgmock properties
        set_target_properties(libgmock PROPERTIES
            "IMPORTED_LOCATION" "${binary_dir}/googlemock/libgmock.a"
            "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
        )
    endif()


    if(WIN32)
        # Create a libgtest target to be used as a dependency by test programs
        add_library(libgtest IMPORTED STATIC GLOBAL)
        add_dependencies(libgtest gtest)

        # Set libgtest properties
        set_target_properties(libgtest PROPERTIES
            "IMPORTED_LOCATION" "${binary_dir}/googlemock/gtest/${CMAKE_CONFIGURATION_TYPES}/gtest.lib"
            "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
        )

        # Create a libgmock target to be used as a dependency by test programs
        add_library(libgmock IMPORTED STATIC GLOBAL)
        add_dependencies(libgmock gtest)

        # Set libgmock properties
        set_target_properties(libgmock PROPERTIES
            "IMPORTED_LOCATION" "${binary_dir}/googlemock/${CMAKE_CONFIGURATION_TYPES}/gmock.lib"
            "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
        )
    endif()


    # I couldn't make it work with INTERFACE_INCLUDE_DIRECTORIES
    include_directories("${source_dir}/googletest/include"
                        "${source_dir}/googlemock/include")

    add_subdirectory(testdf)
