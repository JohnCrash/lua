cmake_minimum_required(VERSION 2.8)

project(lua)

# Include necessary submodules
#set(CMAKE_MODULE_PATH
#	"${iRobot_SOURCE_DIR}/CMake"
#	"${iRobot_SOURCE_DIR}/CMake/Utils"
#	"${iRobot_SOURCE_DIR}/CMake/Packages"
#	"${iRobot_SOURCE_DIR}/CMake/Templates"
#)

set(HEADER_FILES 
	src/lapi.h
	src/lauxlib.h
	src/lcode.h
	src/lctype.h
	src/ldebug.h
	src/ldo.h
	src/lfunc.h
	src/lgc.h
	src/llex.h
	src/llimits.h
	src/lmem.h
	src/lobject.h
	src/lopcodes.h
	src/lparser.h
	src/lstate.h
	src/lstring.h
	src/ltable.h
	src/ltm.h
	src/lua.h
	src/lua.hpp
	src/luaconf.h
	src/lualib.h
	src/lundump.h
	src/lvm.h
	src/lzio.h
	src/lfs/lfs.h
	src/socket/auxiliar.h
	src/socket/buffer.h
	src/socket/except.h
	src/socket/inet.h
	src/socket/io.h
	src/socket/luasocket.h
	src/socket/options.h
	src/socket/select.h
	src/socket/socket.h
	src/socket/tcp.h
	src/socket/timeout.h
	src/socket/udp.h
	src/socket/unix.h
	src/socket/usocket.h
	src/socket/wsocket.h
	src/socket/mime.h
	)

set(SOURCE_FILES 
	src/lapi.c
	src/lauxlib.c
	src/lbaselib.c
	src/lbitlib.c
	src/lcode.c
	src/lcorolib.c
	src/lctype.c
	src/ldblib.c
	src/ldebug.c
	src/ldo.c
	src/ldump.c
	src/lfunc.c
	src/lgc.c
	src/linit.c
	src/liolib.c
	src/llex.c
	src/lmathlib.c
	src/lmem.c
	src/loadlib.c
	src/lobject.c
	src/lopcodes.c
	src/loslib.c
	src/lparser.c
	src/lstate.c
	src/lstring.c
	src/lstrlib.c
	src/ltable.c
	src/ltablib.c
	src/ltm.c
	src/lundump.c
	src/lvm.c
	src/lzio.c
#如果编译成动态库会出问题
#	src/luac.c
#	src/lua.c
	src/lfs/lfs.c
	src/socket/auxiliar.c
	src/socket/buffer.c
	src/socket/except.c
	src/socket/inet.c
	src/socket/io.c
	src/socket/luasocket.c
	src/socket/options.c
	src/socket/select.c
	src/socket/tcp.c
	src/socket/timeout.c
	src/socket/udp.c
	src/socket/unix.c
	src/socket/usocket.c
	src/socket/wsocket.c
	src/socket/mime.c
	)
	
SOURCE_GROUP("Header Files\\lfs" FILES
	src/lfs/lfs.h
)

SOURCE_GROUP("Source Files\\lfs" FILES
	src/lfs/lfs.c
)

SOURCE_GROUP("Header Files\\socket" FILES
	src/socket/auxiliar.h
	src/socket/buffer.h
	src/socket/except.h
	src/socket/inet.h
	src/socket/io.h
	src/socket/luasocket.h
	src/socket/options.h
	src/socket/select.h
	src/socket/socket.h
	src/socket/tcp.h
	src/socket/timeout.h
	src/socket/udp.h
	src/socket/unix.h
	src/socket/usocket.h
	src/socket/wsocket.h
)

SOURCE_GROUP("Source Files\\socket" FILES
	src/socket/auxiliar.c
	src/socket/buffer.c
	src/socket/except.c
	src/socket/inet.c
	src/socket/io.c
	src/socket/luasocket.c
	src/socket/options.c
	src/socket/select.c
	src/socket/tcp.c
	src/socket/timeout.c
	src/socket/udp.c
	src/socket/unix.c
	src/socket/usocket.c
	src/socket/wsocket.c
)

SOURCE_GROUP("Header Files\\mime" FILES
	src/socket/mime.h
)

SOURCE_GROUP("Source Files\\mime" FILES
	src/socket/mime.c
)
add_definitions(/DLUA_COMPAT_MODULE)
# Specify build paths
set(lua_WORK_DIR ${lua_BINARY_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${lua_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${lua_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${lua_BINARY_DIR}/bin)

include_directories(
src
)

option(LUA_STATIC "Static build" FALSE)

if (LUA_STATIC)
	set(LUA_LIB_TYPE STATIC)
else()
	set(LUA_LIB_TYPE SHARED)
endif()

add_library(lua ${LUA_LIB_TYPE} ${HEADER_FILES} ${SOURCE_FILES} ${RESOURCE_FILES})

#编译解释器	
option(LUA_INTERPRETER "Compile lua interpreter" FALSE)
if(LUA_INTERPRETER)
	set(SOURCE_FILES ${SOURCE_FILES} src/lua.c)
	add_executable(luac src/lua.c)
	set(LUA_LIBRARIES lua)
	target_link_libraries(luac ${LUA_LIBRARIES})
	add_dependencies(luac lua)
endif()



