cmake_minimum_required(VERSION 3.5)

project(opkg)

option(USE_LIBSOLV "Link to libsolv instead of internal solver" ON)

if(USE_LIBSOLV)
    # the FindLibSolv.cmake should include this, but currently doesn't
    include(FindPackageHandleStandardArgs)
    find_package(LibSolv REQUIRED)
endif()

include(GNUInstallDirs)
list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/modules")
find_package(Gpgme REQUIRED)
find_package(CURL REQUIRED)
find_package(LibArchive REQUIRED)

add_subdirectory("libopkg")

add_executable("${PROJECT_NAME}" "src/opkg.c")

target_include_directories("${PROJECT_NAME}" PRIVATE
    # TODO we only need this for config.h
    "${PROJECT_SOURCE_DIR}"
    "${PROJECT_SOURCE_DIR}/libopkg"
    )

target_link_libraries("${PROJECT_NAME}" PRIVATE "libopkg")

include("tests/tests.cmake")
