cmake_minimum_required(VERSION 2.8)

project(lua)

option(LUA_BUILD_SHARED "Build shared library" ON)
option(LUA_BUILD_STATIC "Build static library" OFF)
option(LUA_BUILD_EXE "Build lua stand-alone interpreter" OFF)

set(LUA_SRC
	./lapi.c
	./lauxlib.c
	./lbaselib.c
	./lcode.c
    ./lcorolib.c
    ./lctype.c
    ./ldblib.c
    ./ldebug.c
    ./ldo.c
    ./ldump.c
    ./lfunc.c
    ./lgc.c
    ./linit.c
    ./liolib.c
    ./llex.c
    ./lmathlib.c
    ./lmem.c
    ./loadlib.c
    ./lobject.c
    ./lopcodes.c
    ./loslib.c
    ./lparser.c
    ./lstate.c
    ./lstring.c
    ./lstrlib.c
    ./ltable.c
    ./ltablib.c
    #./ltests.c
    ./ltm.c
    ./lundump.c
    ./lutf8lib.c
    ./lvm.c
    ./lzio.c
    ./lfs.c
)

set(LUA_LIB_NAME "unknow")

#静态库
if (LUA_BUILD_STATIC)
    set(LUA_LIB_NAME lua-static)
	add_library(${LUA_LIB_NAME} STATIC ${LUA_SRC})
	target_include_directories(${LUA_LIB_NAME} PUBLIC ./)
endif()

#动态库
if (LUA_BUILD_SHARED)
    if(LUA_BUILD_EXE)
        set(LUA_LIB_NAME lua-shared)
    else()
        set(LUA_LIB_NAME lua)
    endif()
	add_library(${LUA_LIB_NAME} SHARED ${LUA_SRC})
	target_include_directories(${LUA_LIB_NAME} PUBLIC ./)
	if(MSVC)
		set_property(TARGET ${LUA_LIB_NAME} PROPERTY COMPILE_DEFINITIONS LUA_BUILD_AS_DLL=1)
	endif()
	install(TARGETS ${LUA_LIB_NAME} RUNTIME DESTINATION . LIBRARY DESTINATION .)
endif()

if(LUA_BUILD_EXE)
    #解释器
    add_executable(lua lua.h lua.c)
    target_include_directories(lua PRIVATE ./)
    target_link_libraries(lua ${LUA_LIB_NAME})
endif()