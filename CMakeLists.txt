cmake_minimum_required(VERSION 3.16)

set(ObsSourceDir $ENV{OBS_SOURCE_DIR})
set(DepsPath $ENV{OBS_DEPS_DIR})
set(QTDIR ${DepsPath})

set(LIBOBS_INCLUDE_DIR "${ObsSourceDir}/libobs" CACHE FILEPATH "LIBOBS PATH" FORCE)
set(LIBOBS_LIB "${ObsSourceDir}/build64/libobs/Debug/obs.lib" CACHE FILEPATH "OBS library" FORCE)
set(OBS_FRONTEND_LIB "${ObsSourceDir}/build64/UI/obs-frontend-api/Debug/obs-frontend-api.lib" CACHE FILEPATH "OBS frontend library" FORCE)
set(PTHREAD_LIB "${ObsSourceDir}/build64/deps/w32-pthreads/Debug/w32-pthreads.lib" CACHE FILEPATH "PTHREAD library" FORCE)

project(obs-virtualcam)

add_compile_definitions(_CRT_SECURE_NO_WARNINGS)
include_directories("${ObsSourceDir}/deps/w32-pthreads")

find_package(Threads REQUIRED)

if(NOT CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SAFESEH:NO")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /SAFESEH:NO")
    set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} /SAFESEH:NO")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
add_subdirectory(src/virtual-output)
add_subdirectory(src/virtual-source)
