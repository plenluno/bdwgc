# Copyright (c) 2013 Plenluno All rights reserved.

cmake_minimum_required(VERSION 2.8)

project(gc)

# options ----------------------------------------------------------------------

option(BDWGC_BUILD_TEST "Build Tests" OFF)

message(STATUS "BDWGC_BUILD_TEST=${BDWGC_BUILD_TEST}")

# variables --------------------------------------------------------------------

## gc-src
set(gc-src
    allchblk.c
    alloc.c
    backgraph.c
    blacklst.c
    checksums.c
    dbg_mlc.c
    dyn_load.c
    finalize.c
    gc_cpp.cpp
    gc_dlopen.c
    gcj_mlc.c
    headers.c
    mach_dep.c
    malloc.c
    mallocx.c
    mark.c
    mark_rts.c
    misc.c
    new_hblk.c
    obj_map.c
    os_dep.c
    pcr_interface.c
    ptr_chck.c
    real_malloc.c
    reclaim.c
    specific.c
    stubborn.c
    thread_local_alloc.c
    typd_mlc.c
)

if(UNIX)
    set(gc-src
        ${gc-src}
        pthread_start.c
        pthread_support.c
        pthread_stop_world.c
    )

    if(APPLE)
        set(gc-src
            ${gc-src}
            darwin_stop_world.c
        )
    endif(APPLE)
else(UNIX)
    set(gc-src
        ${gc-src}
        win32_threads.c
    )
endif(UNIX)

## gc-test-src
set(gc-test-src
    test/test.c
)

# build ------------------------------------------------------------------------

include_directories(
    include
)

add_library(gc STATIC
    ${gc-src}
)

add_library(gccpp STATIC
    gc_cpp.cpp
)

# test -------------------------------------------------------------------------

if(BDWGC_BUILD_TEST)
    add_executable(libgc-test
        tests/test.c
    )

    target_link_libraries(libgc-test
        gc
    )
endif(BDWGC_BUILD_TEST)
