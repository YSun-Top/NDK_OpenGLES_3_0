cmake_minimum_required(VERSION 3.4.1)

set(jnilibs "${CMAKE_SOURCE_DIR}/../jniLibs")

project(NDK_OpenGLES)
include_directories(
        util
        render
        sample
        glm
        inc
        inc/assimp
        inc/freetype_2_9_1
        model
        opencv_3_0_0
        egl
        looper
)

file(GLOB src-files
        ${CMAKE_SOURCE_DIR}/*.cpp
        ${CMAKE_SOURCE_DIR}/render/*.cpp
        ${CMAKE_SOURCE_DIR}/util/*.cpp
        ${CMAKE_SOURCE_DIR}/sample/*.cpp
        ${CMAKE_SOURCE_DIR}/egl/*.cpp
        ${CMAKE_SOURCE_DIR}/looper/*.cpp)
add_library(
        native-render
        SHARED
        ${src-files})

add_library(assimp SHARED IMPORTED)
set_target_properties(assimp PROPERTIES IMPORTED_LOCATION ${jnilibs}/${ANDROID_ABI}/libassimp.so)

add_library(opencv_java3 SHARED IMPORTED)
set_target_properties(opencv_java3 PROPERTIES IMPORTED_LOCATION ${jnilibs}/${ANDROID_ABI}/libopencv_java3.so)

add_library(freetype STATIC IMPORTED)
set_target_properties(freetype PROPERTIES IMPORTED_LOCATION ${jnilibs}/${ANDROID_ABI}/libfreetype.a)

set(third-party-libs
        freetype
        assimp
        opencv_java3)

target_link_libraries( # Specifies the target library.
        native-render

        # Links the target library to the log library
        # included in the NDK.`
        ${third-party-libs}
        EGL
        GLESv3
        android
        log)

