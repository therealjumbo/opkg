project(opkg_solver_libsolv)

add_library("${PROJECT_NAME}" OBJECT "opkg_solver_libsolv.c")
target_include_directories("${PROJECT_NAME}" PRIVATE
    "${PROJECT_SOURCE_DIR}/../.."
    "${LibSolv_INCLUDE_DIRS}")
