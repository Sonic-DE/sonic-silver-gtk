file(GLOB_RECURSE SCSS_SOURCES "*.scss")
file(GLOB CSS_SOURCES "*.css")
file(GLOB_RECURSE GTK2_SOURCES "gtk2/*")

configure_file(build_theme.sh.cmake ${CMAKE_CURRENT_BINARY_DIR}/build_theme.sh @ONLY)

set(SOURCES
    ${SCSS_SOURCES}
    ${CSS_SOURCES}
    ${GTK2_SOURCES}
    render_assets.py
)

function(gen_targets THEME_NAME)
    set(THEME_DIR "${PROJECT_BINARY_DIR}/${THEME_NAME}")
    set(${THEME_NAME}_TARGETS
        "${THEME_DIR}/gtk-2.0/gtkrc"
        "${THEME_DIR}/gtk-3.0/gtk.css"
        PARENT_SCOPE
    )
endfunction()

gen_targets(Silver)
gen_targets(Silver-Dark)

list(APPEND Silver_TARGETS
    "${PROJECT_BINARY_DIR}/Silver/gtk-3.0/gtk-dark.css"
    "${PROJECT_BINARY_DIR}/Silver/gtk-4.0/gtk-dark.css"
)

add_custom_command(
    OUTPUT ${Silver-Dark_TARGETS}
    DEPENDS ${SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/build_theme.sh
    COMMAND ${CMAKE_CURRENT_BINARY_DIR}/build_theme.sh -c SilverDark -t "${PROJECT_BINARY_DIR}/Silver-Dark" -r "${SILVER_COLOR_INSTALL_ROOT}"
    WORKING_DIRECTORY "${PROJECT_SOURCE_DIR}/src"
)
add_custom_target(Silver-Dark ALL DEPENDS ${Silver-Dark_TARGETS})

add_custom_command(
    OUTPUT ${Silver_TARGETS}
    DEPENDS ${SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/build_theme.sh
    COMMAND ${CMAKE_CURRENT_BINARY_DIR}/build_theme.sh -c SilverLight -t "${PROJECT_BINARY_DIR}/Silver" -r "${SILVER_COLOR_INSTALL_ROOT}"
    COMMAND cp gtk-dark-3.0.css "${PROJECT_BINARY_DIR}/Silver/gtk-3.0/gtk-dark.css"
    COMMAND cp gtk-dark-4.0.css "${PROJECT_BINARY_DIR}/Silver/gtk-4.0/gtk-dark.css"
    WORKING_DIRECTORY "${PROJECT_SOURCE_DIR}/src"
)
add_custom_target(Silver ALL DEPENDS ${Silver_TARGETS})

install(DIRECTORY "${PROJECT_BINARY_DIR}/Silver" DESTINATION "${KDE_INSTALL_FULL_DATAROOTDIR}/themes/")
install(DIRECTORY "${PROJECT_BINARY_DIR}/Silver-Dark" DESTINATION "${KDE_INSTALL_FULL_DATAROOTDIR}/themes/")
install(FILES settings.ini DESTINATION "${KDE_INSTALL_FULL_DATAROOTDIR}/themes/Silver")
install(FILES settings.ini DESTINATION "${KDE_INSTALL_FULL_DATAROOTDIR}/themes/Silver-Dark")
