set(HASHTABLE_SRCS hashtable.c hashtable_itr.c)
set(UNIONFS_SRCS unionfs.c opts.c debug.c findbranch.c readdir.c 
    general.c unlink.c cow.c cow_utils.c string.c rmdir.c usyslog.c)
set(UNIONFSCTL_SRCS unionfsctl.c)

add_executable(unionfs ${UNIONFS_SRCS} ${HASHTABLE_SRCS})

target_link_libraries(unionfs pthread)
if (UNIX AND NOT APPLE)
    target_link_libraries(unionfs rt)
endif()

add_executable(unionfsctl ${UNIONFSCTL_SRCS})

find_package(PkgConfig REQUIRED)
pkg_search_module(FUSE REQUIRED fuse)

target_link_libraries(unionfs ${FUSE_LIBRARIES})
target_include_directories(unionfs PUBLIC ${FUSE_INCLUDE_DIRS})
target_compile_options(unionfs PUBLIC ${FUSE_CFLAGS_OTHER})

INSTALL(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/unionfs DESTINATION bin)
INSTALL(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/unionfsctl DESTINATION bin)
