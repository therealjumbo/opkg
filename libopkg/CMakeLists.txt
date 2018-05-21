project(libopkg)

add_library("${PROJECT_NAME}"
    "pkg_src_list.c"
    "parse_util.c"
    "opkg_cmd.c"
    "opkg_configure.c"
    "opkg.c"
    "pkg_dest_list.c"
    "cksum_list.c"
    "sha256.c"
    "hash_table.c"
    "pkg_src.c"
    "pkg_vec.c"
    "nv_pair.c"
    "opkg_download_curl.c"
    "pkg.c"
    "str_list.c"
    "xfuncs.c"
    "string_util.c"
    "opkg_conf.c"
    "opkg_openssl.c"
    "file_util.c"
    "sprintf_alloc.c"
    "opkg_install.c"
    "pkg_parse.c"
    "opkg_utils.c"
    "opkg_gpg.c"
    "xsystem.c"
    "release_parse.c"
    "opkg_download.c"
    "nv_pair_list.c"
    "pkg_hash.c"
    "opkg_archive.c"
    "void_list.c"
    "md5.c"
    "conffile_list.c"
    "pkg_extract.c"
    "file_list.c"
    "opkg_download_wget.c"
    "opkg_verify.c"
    "pkg_dest.c"
    "opkg_message.c"
    "xregex.c"
    "release.c"
    "conffile.c"
    "opkg_remove.c"
    "pkg_depends.c"
    )

target_include_directories("${PROJECT_NAME}" PRIVATE
    "${PROJECT_SOURCE_DIR}"
    # TODO, we need this dir for config.h, remove config.h
    "${PROJECT_SOURCE_DIR}/.."
    # TODO we need to include the internal solver headers, even if we aren't using it
    "solvers/internal"
    "${LibArchive_INCLUDE_DIRS}"
    )

# TODO, these are all hacks leftover from the autotools build
target_compile_definitions("${PROJECT_NAME}" PRIVATE
    "SYSCONFDIR=\"${CMAKE_INSTALL_SYSCONFDIR}\""
    "VARDIR=\"${CMAKE_INSTALL_LOCALSTATEDIR}\""
    "DATADIR=\"${CMAKE_INSTALL_DATADIR}\""
    "HOST_CPU_STR=__${CMAKE_SYSTEM_PROCESSOR}"
    )

target_link_libraries("${PROJECT_NAME}" PUBLIC
    "${LibArchive_LIBRARIES}"
    "${CURL_LIBRARIES}"
    "${GPGME_VANILLA_LIBRARIES}"
    "-Wl,--no-undefined"
    )

target_include_directories("${PROJECT_NAME}" PRIVATE "solvers/internal")

if(USE_LIBSOLV)
    add_subdirectory("solvers/libsolv")
    target_sources("${PROJECT_NAME}" PRIVATE $<TARGET_OBJECTS:opkg_solver_libsolv>)
    # TODO, we should need a header for solvers/libsolv, currently there is no header
    target_link_libraries("${PROJECT_NAME}" PUBLIC "${LIBSOLV_LIBRARY}")
else()
    add_subdirectory("solvers/internal")
    target_sources("${PROJECT_NAME}" PRIVATE $<TARGET_OBJECTS:opkg_solver_internal>)
endif()
