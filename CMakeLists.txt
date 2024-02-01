cmake_minimum_required(VERSION 3.6)

project(DDSHelloWorld)

find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)

set (CMAKE_CXX_STANDARD 17)

message(STATUS "Configuring HelloWorld publisher/subscriber example...")


# Create a library for the message / topic
add_custom_target(
    messages
    COMMAND fastddsgen HelloWorldMsg.idl
)
add_library(HellowWorldMsg STATIC HelloWorldMsg.cxx HelloWorldMsgPubSubTypes.cxx)
add_dependencies(HellowWorldMsg messages)


# Publisher
add_executable(DDSHelloWorldPublisher HelloWorldPublisher.cpp)
target_link_libraries(DDSHelloWorldPublisher fastcdr fastrtps HellowWorldMsg)


# Subscriber
add_executable(DDSHelloWorldSubscriber HelloWorldSubscriber.cpp)
target_link_libraries(DDSHelloWorldSubscriber fastcdr fastrtps HellowWorldMsg)
