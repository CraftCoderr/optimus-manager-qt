cmake_minimum_required(VERSION 3.7.0)

project(optimus-manager-qt VERSION 1.5.3 LANGUAGES CXX DESCRIPTION "An interface for Optimus Manager that allows to switch GPUs on Optimus laptops.")

set(APPLICATION_NAME "Optimus Manager Qt")
set(ORGANIZATION_NAME "Optimus Manager")
set(DESKTOP_FILE io.optimus_manager.OptimusManagerQt.desktop)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

option(WITH_PLASMA "Use additional KDE API feautures")

find_package(ECM REQUIRED NO_MODULE)
list(APPEND CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

find_package(Qt5 5.10 COMPONENTS Widgets DBus X11Extras LinguistTools REQUIRED)
find_package(X11 COMPONENTS Xrandr REQUIRED)
if(WITH_PLASMA)
    find_package(KF5 COMPONENTS Notifications IconThemes REQUIRED)
endif()

include(cmake/ExternalLibraries.cmake)
include(ECMInstallIcons)
include(GNUInstallDirs)

qt5_add_translation(QM_FILES
    data/translations/${PROJECT_NAME}_de_DE.ts
    data/translations/${PROJECT_NAME}_es_ES.ts
    data/translations/${PROJECT_NAME}_fi_FI.ts
    data/translations/${PROJECT_NAME}_hu_HU.ts
    data/translations/${PROJECT_NAME}_pt_BR.ts
    data/translations/${PROJECT_NAME}_ro_RO.ts
    data/translations/${PROJECT_NAME}_ru_RU.ts
    data/translations/${PROJECT_NAME}_tr_TR.ts
    data/translations/${PROJECT_NAME}_zh_CN.ts
)

configure_file(src/cmake.h.in cmake.h)

add_executable(${PROJECT_NAME}
    ${QM_FILES}
    data/icons/flags/flags.qrc
    src/appsettings.cpp
    src/daemonclient.cpp
    src/main.cpp
    src/optimusmanager.cpp
    src/optimussettings.cpp
    src/settingsdialog.cpp
    src/settingsdialog.ui
    src/x11deleters.cpp
)

target_link_libraries(${PROJECT_NAME} PRIVATE SingleApplication::SingleApplication Qt5::DBus Qt5::X11Extras X11::Xrandr)
if(WITH_PLASMA)
    target_link_libraries(${PROJECT_NAME} PRIVATE KF5::Notifications KF5::IconThemes)
    target_compile_definitions(${PROJECT_NAME} PRIVATE WITH_PLASMA)
endif()

install(TARGETS ${PROJECT_NAME})
install(FILES ${QM_FILES} DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${ORGANIZATION_NAME}/${APPLICATION_NAME}/translations)
install(FILES data/${DESKTOP_FILE} DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/applications)

ecm_install_icons(ICONS
    data/icons/freedesktop/128-apps-amd.svg
    data/icons/freedesktop/128-apps-intel.svg
    data/icons/freedesktop/128-apps-nvidia.svg
    data/icons/freedesktop/128-apps-optimus-manager.svg
    data/icons/freedesktop/16-apps-amd.svg
    data/icons/freedesktop/16-apps-intel.svg
    data/icons/freedesktop/16-apps-nvidia.svg
    data/icons/freedesktop/16-apps-optimus-manager.svg
    data/icons/freedesktop/16-status-prime-amd.svg
    data/icons/freedesktop/16-status-prime-hybrid.svg
    data/icons/freedesktop/16-status-prime-intel.svg
    data/icons/freedesktop/16-status-prime-nvidia.svg
    data/icons/freedesktop/22-apps-amd.svg
    data/icons/freedesktop/22-apps-intel.svg
    data/icons/freedesktop/22-apps-nvidia.svg
    data/icons/freedesktop/22-apps-optimus-manager.svg
    data/icons/freedesktop/22-status-prime-amd.svg
    data/icons/freedesktop/22-status-prime-hybrid.svg
    data/icons/freedesktop/22-status-prime-intel.svg
    data/icons/freedesktop/22-status-prime-nvidia.svg
    data/icons/freedesktop/24-apps-amd.svg
    data/icons/freedesktop/24-apps-intel.svg
    data/icons/freedesktop/24-apps-nvidia.svg
    data/icons/freedesktop/24-apps-optimus-manager.svg
    data/icons/freedesktop/24-status-prime-amd.svg
    data/icons/freedesktop/24-status-prime-hybrid.svg
    data/icons/freedesktop/24-status-prime-intel.svg
    data/icons/freedesktop/24-status-prime-nvidia.svg
    data/icons/freedesktop/256-apps-amd.svg
    data/icons/freedesktop/256-apps-intel.svg
    data/icons/freedesktop/256-apps-nvidia.svg
    data/icons/freedesktop/256-apps-optimus-manager.svg
    data/icons/freedesktop/48-apps-amd.svg
    data/icons/freedesktop/48-apps-intel.svg
    data/icons/freedesktop/48-apps-nvidia.svg
    data/icons/freedesktop/48-apps-optimus-manager.svg
    data/icons/freedesktop/512-apps-amd.svg
    data/icons/freedesktop/512-apps-intel.svg
    data/icons/freedesktop/512-apps-nvidia.svg
    data/icons/freedesktop/512-apps-optimus-manager.svg
    data/icons/freedesktop/64-apps-amd.svg
    data/icons/freedesktop/64-apps-intel.svg
    data/icons/freedesktop/64-apps-nvidia.svg
    data/icons/freedesktop/64-apps-optimus-manager.svg
    data/icons/freedesktop/96-apps-amd.svg
    data/icons/freedesktop/96-apps-intel.svg
    data/icons/freedesktop/96-apps-nvidia.svg
    data/icons/freedesktop/96-apps-optimus-manager.svg
    data/icons/freedesktop/sc-apps-amd.svg
    data/icons/freedesktop/sc-apps-intel.svg
    data/icons/freedesktop/sc-apps-nvidia.svg
    data/icons/freedesktop/sc-apps-optimus-manager.svg
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/icons
)
