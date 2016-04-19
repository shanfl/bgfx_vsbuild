project(bgfx)

macro(source_group_by_dir source_files)
    if(MSVC)
        set(sgbd_cur_dir ${CMAKE_CURRENT_SOURCE_DIR})
        foreach(sgbd_file ${${source_files}})
            string(REGEX REPLACE ${sgbd_cur_dir}/\(.*\) \\1 sgbd_fpath ${sgbd_file})
            string(REGEX REPLACE "\(.*\)/.*" \\1 sgbd_group_name ${sgbd_fpath})
            string(COMPARE EQUAL ${sgbd_fpath} ${sgbd_group_name} sgbd_nogroup)
            string(REPLACE "/" "\\" sgbd_group_name ${sgbd_group_name})
            if(sgbd_nogroup)
                set(sgbd_group_name "\\")
            endif(sgbd_nogroup)
            source_group(${sgbd_group_name} FILES ${sgbd_file})
        endforeach(sgbd_file)
    endif(MSVC)
endmacro(source_group_by_dir)


include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bx)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bx/include)
IF(MSVC)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bx/include/compat/msvc)
ENDIF(MSVC)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/khronos)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/dxsdk/include)

IF(MSVC)
#add_definitions(-DBGFX_CONFIG_RENDERER_DIRECT3D12=0)
ENDIF(MSVC)


FILE(GLOB_RECURSE INC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/include/*.h)
FILE(GLOB_RECURSE SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/src/*.cpp)
FILE(GLOB_RECURSE SRC_H_FILES ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/src/*.h)
FILE(GLOB_RECURSE SRC_SC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/src/*.sc)
#FILE(GLOB_RECURSE THIRD_CPP ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/*.cpp)
#FILE(GLOB_RECURSE THIRD_C ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/*.c)
#FILE(GLOB_RECURSE THIRD_H ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/*.h)

SET(ALL_SRC ${INC_FILES} ${SRC_FILES} ${SRC_H_FILES} ${SRC_SC_FILES})
source_group_by_dir(ALL_SRC)

ADD_LIBRARY(bgfx  STATIC ${ALL_SRC})
#TARGET_LINK_LIBRARIES(bgfx Psapi.lib)

ADD_DEFINITIONS(-DSTB_IMAGE_STATIC)
#common.lib
set(src_common 
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/cube_atlas.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/camera.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/bounds.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/bgfx_utils.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/bgfx_utils.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/entry/entry_windows.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/entry/entry.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/entry/input.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/entry/cmd.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common/entry/dbg.cpp

)
ADD_LIBRARY(common  STATIC ${src_common})



#3rd lib

FILE(GLOB ib_compress_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/ib-compress/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/ib-compress/*.cpp)
FILE(GLOB edtaa3_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/edtaa3/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/edtaa3/*.cpp)
FILE(GLOB etc1_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/etc1/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/etc1/*.cpp)
FILE(GLOB etc2_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/etc2/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/etc2/*.cpp)
FILE(GLOB forsyth-too_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/forsyth-too/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/forsyth-too/*.cpp)
FILE(GLOB stb_src  ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/stb/*.h ${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/stb/*.c)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/3rdparty/stb)
set(THIRD_SRC 
		${ib_compress_src}
		${edtaa3_src}
		${etc1_src}
		${etc2_src}
		${forsyth}
		${stb_src}
)

add_library(3rd STATIC ${THIRD_SRC})

# EXAMPLE 00

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/common)
SET(helloworld 
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/00-helloworld/helloworld.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/bgfx/examples/00-helloworld/logo.h
	)
	
add_executable(helloworld ${helloworld})
TARGET_LINK_LIBRARIES(helloworld Psapi.lib common bgfx 3rd)

