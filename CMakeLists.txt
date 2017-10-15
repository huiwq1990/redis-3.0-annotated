# vim: set ai expandtab:

cmake_minimum_required(VERSION 3.2 FATAL_ERROR)

# The following must be set BEFORE doing project() or eanble_language().
# ::-------------------------------------------------------------------------::
message("j")
if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type defined; defaulting to 'Debug'")
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING
            "The type of build. Possible values are: Debug, Release, RelWithDebInfo and MinSizeRel.")
endif()
# ::-------------------------------------------------------------------------::

project(redis3.0)

message(STATUS "Host is: ${CMAKE_HOST_SYSTEM}.  Build target is: ${CMAKE_SYSTEM}")
get_filename_component(REDIS_ROOT "${CMAKE_CURRENT_SOURCE_DIR}" ABSOLUTE)
message(STATUS "Project root directory is: ${REDIS_ROOT}")

if (false)
    message("C++ compiler supports these language features:")
    foreach(i ${CMAKE_CXX_COMPILE_FEATURES})
        message("  ${i}")
    endforeach()
endif()

add_executable(redis3.0
        src/adlist.h
        src/ae.h
        src/anet.h
        src/asciilogo.h
        src/bio.h
        src/cluster.h
        src/config.h
        src/crc64.h
        src/dict.h
        src/endianconv.h
        src/fmacros.h
        src/help.h
        src/intset.h
        src/lzf.h
        src/lzfP.h
        src/pqsort.h
        src/rand.h
        src/rdb.h
        src/redis.h
        src/redisassert.h
        src/release.h
        src/rio.h
        src/sds.h
        src/sha1.h
        src/slowlog.h
        src/solarisfixes.h
        src/testhelp.h
        src/util.h
        src/version.h
        src/ziplist.h
        src/zipmap.h
        src/zmalloc.h
        src/adlist.c
        src/ae.c
#        src/ae_epoll.c
#        src/ae_evport.c
#        src/ae_kqueue.c
#        src/ae_select.c
        src/anet.c
        src/aof.c
        src/bio.c
        src/bitops.c
        src/blocked.c
        src/cluster.c
        src/config.c
        src/crc16.c
        src/crc64.c
        src/db.c
        src/debug.c
        src/dict.c
        src/endianconv.c
        src/hyperloglog.c
        src/intset.c
        src/lzf_c.c
        src/lzf_d.c
        src/memtest.c
        src/multi.c
        src/networking.c
        src/notify.c
        src/object.c
        src/pqsort.c
        src/pubsub.c
        src/rand.c
        src/rdb.c
        src/redis-benchmark.c
        src/redis-check-aof.c
        src/redis-check-dump.c
        src/redis-cli.c
        src/redis.c
        src/release.c
        src/replication.c
        src/rio.c
        src/scripting.c
        src/sds.c
        src/sentinel.c
        src/setproctitle.c
        src/sha1.c
        src/slowlog.c
        src/sort.c
        src/syncio.c
        src/t_hash.c
        src/t_list.c
        src/t_set.c
        src/t_string.c
        src/t_zset.c
        src/util.c
        src/ziplist.c
        src/zipmap.c
        src/zmalloc.c
        )

target_include_directories(redis3.0
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/hiredis
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/linenoise
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/lua/src
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/geohash-int
        )

# now create an imported static target
add_library(luaa STATIC IMPORTED)
# Import target "scintilla" for configuration ""
set_property(TARGET luaa APPEND PROPERTY IMPORTED_CONFIGURATIONS NOCONFIG)
set_target_properties(luaa PROPERTIES IMPORTED_LOCATION_NOCONFIG "${REDIS_ROOT}/deps/lua/src/liblua.a")

target_link_libraries(redis3.0
        PRIVATE m
        PRIVATE luaa
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/lua/src/liblua.a
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/linenoise/linenoise.o
        PRIVATE ${PROJECT_SOURCE_DIR}/deps/hiredis/libhiredis.a
        )

message(${PROJECT_SOURCE_DIR})
message(${PROJECT_BINARY_DIR})


add_custom_target(redis-server COMMAND make -C ${PROJECT_SOURCE_DIR}
        CLION_EXE_DIR=${PROJECT_BINARY_DIR} && cp ${PROJECT_SOURCE_DIR}/src/redis-server ./
        )