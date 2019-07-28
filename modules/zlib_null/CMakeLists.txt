project(zlib_null)

include(PCHSupport)
include(CRTLinkage)

set(SRC_DIR ${CMAKE_CURRENT_LIST_DIR}/src)

file(GLOB PROJECT_SOURCES ${SRC_DIR}/*.cpp)
list(REMOVE_ITEM PROJECT_SOURCES ${SRC_DIR}/pch.cpp)

set(INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/include)
file(GLOB PROJECT_HEADERS ${INCLUDE_DIR}/*.h)

set(MISC
	${SRC_DIR}/pch.h
	${SRC_DIR}/pch.cpp
	)

add_definitions(
	-DUNICODE
	-D_UNICODE
	)

source_group("Header Files" FILES ${PROJECT_HEADERS})
source_group("Source Files" FILES ${PROJECT_SOURCES})
source_group("" FILES ${MISC})

add_executable(${PROJECT_NAME}
	${PROJECT_SOURCES}
	${PROJECT_HEADERS}
	${MISC}
	)

add_precompiled_headers(${PROJECT_NAME} "src/")

include_directories(
	${INCLUDE_DIR}
	${ZLIB_INCLUDE_DIR}
	)

target_link_libraries(${PROJECT_NAME}
	${ZLIB_LIB}
	)

add_dependencies(${PROJECT_NAME}
	zlib
	)

#set_target_properties(${PROJECT_NAME} PROPERTIES EXCLUDE_FROM_DEFAULT_BUILD 1)