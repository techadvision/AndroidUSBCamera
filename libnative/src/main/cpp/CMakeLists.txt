
cmake_minimum_required(VERSION 3.10.2)


project("nativelib")

# lame
include_directories(${CMAKE_SOURCE_DIR}/module/mp3/lame)
aux_source_directory(${CMAKE_SOURCE_DIR}/module/mp3/lame SRC_LAME)

add_library(
        nativelib
        SHARED

        ${SRC_LAME}
        utils/logger.cpp
        module/yuv/yuv.cpp
        module/mp3/mp3.cpp
        proxy/proxy_yuv.cpp
        proxy/proxy_mp3.cpp
        nativelib.cpp)

find_library(
        log-lib
        log)

target_link_libraries(
        nativelib
        ${log-lib})

# ✅ 16 KB page-size for Android 16KB devices
target_link_options(nativelib PRIVATE
        "-Wl,-z,max-page-size=16384"
        "-Wl,-z,common-page-size=16384"
)