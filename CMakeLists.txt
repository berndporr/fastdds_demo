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

# Create a library for the message
add_custom_target(
    messages
    COMMAND fastddsgen HelloWorldMsg.idl
)
add_library(HellowWorldMsg STATIC HelloWorldMsg.cxx HelloWorldMsgPubSubTypes.cxx)
add_dependencies(HellowWorldMsg messages)

add_executable(DDSHelloWorldPublisher HelloWorldPublisher.cpp)

target_link_libraries(DDSHelloWorldPublisher fastcdr fastrtps HellowWorldMsg)



add_executable(DDSHelloWorldSubscriber HelloWorldSubscriber.cpp)

target_link_libraries(DDSHelloWorldSubscriber fastcdr fastrtps HellowWorldMsg)
