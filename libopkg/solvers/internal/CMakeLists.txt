project(opkg_solver_internal)

add_library("${PROJECT_NAME}" OBJECT
    "pkg_depends_internal.c"
    "opkg_action.c"
    "opkg_install_internal.c"
    "opkg_upgrade_internal.c"
    "opkg_solver_internal.c"
    )
