cmake_minimum_required(VERSION 2.8)

project(helori C CXX)

enable_testing()

find_package(GnuTLS REQUIRED)

file(STRINGS VERSION HELORI_VERSION)

message("helori version: ${HELORI_VERSION}")
message("PREFIX: ${CMAKE_INSTALL_PREFIX}")

add_definitions(
  -DINSTALL_PREFIX="${CMAKE_INSTALL_PREFIX}"
  -std=c++0x
  -D_GNU_SOURCE -D_LARGEFILE64_SOURCE
  -Wall -Wextra -Werror=return-type
  -DHELORI_VERSION="${HELORI_VERSION}"
  ${GNUTLS_DEFINITIONS})

set(CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} -std=c99")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

add_subdirectory(mimosa EXCLUDE_FROM_ALL)
include_directories(mimosa)

include(CheckIncludeFiles)
include(CheckLibraryExists)

add_subdirectory(src)

install(
  DIRECTORY www/
  DESTINATION share/helori/www
  PATTERN *.less EXCLUDE
  PATTERN update.sh EXCLUDE)
