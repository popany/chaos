Example 1

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
    
    find_package(Boost 1.61.0 REQUIRED thread)
    
    list(APPEND CMAKE_LIBRARY_PATH $ENV{FOO_LIB_PATH})
    find_library(FOO_LIB foo)
    
    # debug
    # include(CMakePrintHelpers)
    # cmake_print_variables(FOO_LIB CMAKE_LIBRARY_PATH)
    
    target_link_libraries(appname
        PRIVATE
            Boost::thread
            ${FOO_LIB}
    )
    
