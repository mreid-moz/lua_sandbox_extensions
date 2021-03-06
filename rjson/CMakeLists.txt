# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, You can obtain one at http://mozilla.org/MPL/2.0/.

cmake_minimum_required(VERSION 3.0)
project(rjson VERSION 1.0.0 LANGUAGES C CXX)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "RapidJSON Lua module")

include(ExternalProject)
externalproject_add(
    rapidjson
    GIT_REPOSITORY https://github.com/miloyip/rapidjson.git
    GIT_TAG 1c087b77cb7eb40a40aaf1d0f28feedc2d6e42ee
    CONFIGURE_COMMAND ${CMAKE_COMMAND} -E echo "no configure"
    BUILD_COMMAND ${CMAKE_COMMAND} -E echo "no build"
    INSTALL_COMMAND ${CMAKE_COMMAND} -E echo "no install"
)
include_directories(${CMAKE_BINARY_DIR}/${PROJECT_NAME}/rapidjson-prefix/src/rapidjson/include)

set(MODULE_SRCS rjson.cpp rjson.def)
set(CPACK_DEBIAN_PACKAGE_DEPENDS "luasandbox (>= 1.0), libc6 (>= 2.14), libgcc1 (>= 1:4.1.1), libstdc++6 (>= 4.1.1)")
set(INSTALL_MODULE_PATH ${INSTALL_IOMODULE_PATH})
if(NOT LUA51)
    find_package(ZLIB)
    if(ZLIB_FOUND)
      add_definitions(-DHAVE_ZLIB)
	  set(CPACK_DEBIAN_PACKAGE_DEPENDS "${CPACK_DEBIAN_PACKAGE_DEPENDS}, zlib1g (>= 1:1.1.4)")
    endif()
endif()

include(sandbox_module)
add_dependencies(rjson rapidjson)

if(ZLIB_FOUND)
  target_link_libraries(rjson ${ZLIB_LIBRARIES})
endif()
