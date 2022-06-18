project(tech.getflow.flappyAurora CXX)
cmake_minimum_required(VERSION 3.5)

find_package (Qt5 COMPONENTS Core Network Qml Gui Quick LinguistTools REQUIRED)

include(FindPkgConfig)
pkg_search_module(SAILFISH sailfishapp REQUIRED)
pkg_search_module(SAILFISH sailfishapp_i18n REQUIRED)

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)


set(short_name flappyAurora)

FILE(GLOB TsFiles "translations/*.ts")
qt5_add_translation(QmFiles ${TsFiles})

add_executable(tech.getflow.flappyAurora src/main.cpp ${QmFiles})
target_compile_definitions(tech.getflow.flappyAurora PRIVATE
    $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>
)
target_include_directories(tech.getflow.flappyAurora PRIVATE
    $<BUILD_INTERFACE:
    ${SAILFISH_INCLUDE_DIRS}
>)
target_link_libraries(tech.getflow.flappyAurora
    Qt5::Quick
    ${SAILFISH_LDFLAGS}
)

install(TARGETS tech.getflow.flappyAurora
    RUNTIME DESTINATION bin
)
install(DIRECTORY qml
    DESTINATION share/tech.getflow.flappyAurora
)

foreach(_file IN LISTS QmFiles)
    get_filename_component(_filename "${_file}" NAME)
    string(REPLACE "${short_name}" "${PROJECT_NAME}" _full_filename "${_filename}")
    install(FILES ${_file} DESTINATION share/tech.getflow.flappyAurora/translations RENAME ${_full_filename})
endforeach()

install(FILES tech.getflow.flappyAurora.desktop
    DESTINATION share/applications
)
install(FILES icons/86x86/tech.getflow.flappyAurora.png
    DESTINATION share/icons/hicolor/86x86/apps
)
install(FILES icons/108x108/tech.getflow.flappyAurora.png
    DESTINATION share/icons/hicolor/108x108/apps
)
install(FILES icons/128x128/tech.getflow.flappyAurora.png
    DESTINATION share/icons/hicolor/128x128/apps
)
install(FILES icons/172x172/tech.getflow.flappyAurora.png
    DESTINATION share/icons/hicolor/172x172/apps
)

# Get the other files reachable from the project tree in Qt Creator
FILE(GLOB TsFiles "translations/*.ts")
add_custom_target(distfiles
    SOURCES
        AUTHORS.md
        CODE_OF_CONDUCT.md
        CONTRIBUTING.md
        LICENSE.BSD-3-CLAUSE.md
        README.md
        tech.getflow.flappyAurora.desktop
        qml/flappyAurora.qml
        qml/cover/DefaultCoverPage.qml
        qml/pages/MainPage.qml
        qml/pages/AboutPage.qml
        qml/components/Main.qml
        qml/components/Pipe.qml
        qml/components/Font.qml
        rpm/tech.getflow.flappyAurora.spec
        rpm/tech.getflow.flappyAurora.yaml
        rpm/tech.getflow.flappyAurora.changes.in
        rpm/tech.getflow.flappyAurora.changes.run.in
        ${TsFiles})

file(WRITE "${CMAKE_BINARY_DIR}/QtCreatorDeployment.txt"
    "${CMAKE_INSTALL_PREFIX}
${CMAKE_BINARY_DIR}/tech.getflow.flappyAurora:bin
")

