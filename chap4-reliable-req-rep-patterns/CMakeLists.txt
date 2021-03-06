cmake_minimum_required(VERSION 2.6)
project(chap2-sockets-and-patterns)

# Setup build-related defaults
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()
if(NOT CMAKE_INSTALL_PREFIX)
    set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/install)
endif()
if(NOT CMAKE_RUNTIME_OUTPUT_DIRECTORY)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/runtime)
endif()

get_filename_component(DEPENDS_DIR ${CMAKE_SOURCE_DIR}/depends ABSOLUTE)
list(APPEND CMAKE_MODULE_PATH ${DEPENDS_DIR}/cmake)

find_package(ZeroMQ REQUIRED)
find_package(CZMQ REQUIRED)

include_directories(
    .
    ..
    ${ZEROMQ_INCLUDE_DIRS}
    ${CZMQ_INCLUDE_DIRS}
)

set(LIBS
    ${ZEROMQ_LIBRARIES}
    ${CZMQ_LIBRARIES}
)

# Lazy-Pirate
add_executable(lpclient lpclient.cpp)
target_link_libraries(lpclient ${LIBS})

add_executable(lpserver lpserver.cpp)
target_link_libraries(lpserver ${LIBS})

# Simple-Pirate queue
add_executable(spqueue spqueue.cpp)
target_link_libraries(spqueue ${LIBS})

add_executable(spworker spworker.cpp)
target_link_libraries(spworker ${LIBS})

# Paranoid-Pirate
add_executable(ppqueue ppqueue.c)
target_link_libraries(ppqueue ${LIBS})

add_executable(ppworker ppworker.c)
target_link_libraries(ppworker ${LIBS})

# Majordomo
add_library(mdcli majordomo/mdcliapi.h majordomo/mdcliapi.c)
add_executable(mdclient majordomo/mdclient.c)
target_link_libraries(mdclient mdcli ${LIBS})

add_library(mdwrk majordomo/mdwrkapi.h majordomo/mdwrkapi.c)
add_executable(mdworker majordomo/mdworker.c)
target_link_libraries(mdworker mdwrk ${LIBS})
