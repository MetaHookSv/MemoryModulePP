cmake_minimum_required(VERSION 3.21)
project(MemoryModulePP LANGUAGES CXX)

if(NOT WIN32 OR NOT MSVC OR NOT CMAKE_SIZEOF_VOID_P EQUAL 4)
    message(FATAL_ERROR "MemoryModulePP currently requires Windows MSVC x86.")
endif()

add_library(MemoryModule STATIC
    MemoryModule/Initialize.cpp
    MemoryModule/LoadDllMemoryApi.cpp
    MemoryModule/MemoryModule.cpp
    MemoryModule/MmpDotNet.cpp
    MemoryModule/MmpLdrpTls.cpp
    MemoryModule/MmpTls.cpp
    MemoryModule/Loader.cpp
    MemoryModule/InvertedFunctionTable.cpp
    MemoryModule/LdrEntry.cpp
    MemoryModule/BaseAddressIndex.cpp
    MemoryModule/Utils.cpp
)
target_compile_features(MemoryModule PUBLIC cxx_std_20)
target_include_directories(MemoryModule PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_compile_definitions(MemoryModule PRIVATE WIN32 _LIB MMPP_EXPORTS UNICODE _UNICODE
    $<$<CONFIG:Debug>:_DEBUG> $<$<NOT:$<CONFIG:Debug>>:NDEBUG>)
target_compile_options(MemoryModule PRIVATE /W3 /sdl /permissive-
    $<$<CONFIG:Release>:/Gy> $<$<CONFIG:Release>:/Oi> $<$<CONFIG:Release>:/guard:cf>)
set_target_properties(MemoryModule PROPERTIES
    MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
    INTERPROCEDURAL_OPTIMIZATION_RELEASE TRUE
)
target_link_libraries(MemoryModule PUBLIC ntdll)
