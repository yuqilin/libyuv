cmake_minimum_required(VERSION 3.4.1)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

set(yuv_files
        ${CMAKE_CURRENT_SOURCE_DIR}/source/compare.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/compare_common.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/compare_posix.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert_argb.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert_from.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert_from_argb.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert_to_argb.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/convert_to_i420.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/cpu_id.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/format_conversion.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/planar_functions.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/rotate.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/rotate_argb.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/rotate_mips.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/row_any.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/row_common.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/row_mips.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/row_posix.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/scale.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_argb.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_common.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_mips.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_posix.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/source/video_common.cc)

if(${ANDROID_ABI} STREQUAL armeabi-v7a)
  #  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DLIBYUV_NEON")
  set(neon_SRCS
      ${CMAKE_CURRENT_SOURCE_DIR}/source/compare_neon.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/rotate_neon.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/row_neon.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_neon.cc)

  set_property(SOURCE ${neon_SRCS}
          APPEND_STRING PROPERTY COMPILE_FLAGS " -mfpu=neon")
  add_definitions("-DLIBYUV_NEON")

elseif(${ANDROID_ABI} STREQUAL arm64-v8a)
#  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DLIBYUV_NEON")
  set(neon_SRCS
      ${CMAKE_CURRENT_SOURCE_DIR}/source/compare_neon64.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/rotate_neon64.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/row_neon64.cc
      ${CMAKE_CURRENT_SOURCE_DIR}/source/scale_neon64.cc)
  set_property(SOURCE ${neon_SRCS}
          APPEND_STRING PROPERTY COMPILE_FLAGS " -mfpu=neon")
  add_definitions("-DLIBYUV_NEON")

endif()

add_library(yuv_static
        STATIC
        ${yuv_files}
        ${neon_SRCS})