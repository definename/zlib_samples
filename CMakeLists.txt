cmake_minimum_required(VERSION 3.7.0)

project(zlib_samples)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
option(BUILD_WITH_STATIC_CRT "Use static run-time libraries (/MT or /MTd linker flags)" OFF)
option(BUILD_SHARED_LIBS "Build shared libraries (.dll) instead of static ones (.lib)" OFF)

# Sets executable output folder
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

include(ExternalProject)

#External directory path
set(EXTERNAL_PATH ${CMAKE_SOURCE_DIR}/external)
#Patch directory path
set(PATCH_PATH ${EXTERNAL_PATH}/patches)
#External install directory path
set(EXTERNAL_INSTALL_PATH ${CMAKE_BINARY_DIR}/external)
#Zlib root directory
set(ZLIB_ROOT ${EXTERNAL_INSTALL_PATH}/ZLIB)

ExternalProject_Add(
	zlib
	URL "${EXTERNAL_PATH}/zlib-1.2.8.zip"
	URL_MD5 D34BAA0DA16E33A5EF095E7EEF7D2472
	CMAKE_ARGS
		-DCMAKE_INSTALL_PREFIX:PATH=${ZLIB_ROOT}
		-DBUILD_SHARED_LIBS:BOOL=${BUILD_SHARED_LIBS}
	PATCH_COMMAND 
		${CMAKE_COMMAND} -P ${PATCH_PATH}/zlib-1.2.8.patches/patch.cmake ${CMAKE_SOURCE_DIR}
	)

set(ZLIB_INCLUDE_DIR ${ZLIB_ROOT}/include)
set(ZLIB_LIB
	debug ${ZLIB_ROOT}/lib/zlibstaticd.lib
	optimized ${ZLIB_ROOT}/lib/zlibstatic.lib
	)

# Use folder in project
SET_PROPERTY(GLOBAL PROPERTY USE_FOLDERS ON)
SET_PROPERTY(TARGET zlib PROPERTY FOLDER "External")

add_subdirectory(modules/zlib_null)