cmake_minimum_required(VERSION 3.20)

project(DDSHelloWorld)

find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)

# Set C++11
include(CheckCXXCompilerFlag)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG OR
        CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    check_cxx_compiler_flag(-std=c++11 SUPPORTS_CXX11)
    if(SUPPORTS_CXX11)
        add_compile_options(-std=c++11)
    else()
        message(FATAL_ERROR "Compiler doesn't support C++11")
    endif()
endif()

message(STATUS "Configuring HelloWorld publisher/subscriber example...")

file(GLOB DDS_HELLOWORLD_SOURCES_CXX "*.cxx")

add_executable(DDSHelloWorldPublisher HelloWorldPublisher.cpp ${DDS_HELLOWORLD_SOURCES_CXX})

target_link_libraries(DDSHelloWorldPublisher fastcdr fastrtps)



add_executable(DDSHelloWorldSubscriber HelloWorldSubscriber.cpp ${DDS_HELLOWORLD_SOURCES_CXX})

target_link_libraries(DDSHelloWorldSubscriber fastrtps fastcdr)
