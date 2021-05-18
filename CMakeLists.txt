cmake_minimum_required(VERSION 3.15)

project(wkhtmltopdf VERSION 0.12.6)

include(ExternalProject)
include(GNUInstallDirs)

set(SRC_PREFIX .wkhtmltopdf)

ExternalProject_Add(wkhtmltopdf_qmake
    PREFIX ${SRC_PREFIX}
    GIT_REPOSITORY git@github.com:dmott-eyelock/wkhtmltopdf.git
    GIT_TAG 0.12.6
    GIT_SHALLOW TRUE
    GIT_SUBMODULES ""
    GIT_SUBMODULES_RECURSE FALSE
    PATCH_COMMAND git apply ${PROJECT_SOURCE_DIR}/msys2_mingw.patch
    UPDATE_COMMAND ""
    CONFIGURE_COMMAND qmake
    BUILD_COMMAND make
    BUILD_IN_SOURCE TRUE
    INSTALL_COMMAND ""
    TEST_COMMAND ""
)

set(_build_dir ${PROJECT_BINARY_DIR}/${SRC_PREFIX}/src/wkhtmltopdf_qmake)

install(FILES 
    ${_build_dir}/bin/wkhtmltoimage.exe
    ${_build_dir}/bin/wkhtmltopdf.exe
    ${_build_dir}/bin/wkhtmltox.dll
    DESTINATION ${CMAKE_INSTALL_BINDIR}
)

set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS)

macro(find_install_lib _name)
    find_file(${_name} ${_name} REQUIRED)
    get_filename_component(${_name} ${${_name}} ABSOLUTE)
    message("installing: ${_name}")
    install(FILES ${${_name}} DESTINATION ${CMAKE_INSTALL_BINDIR})
endmacro()

find_install_lib(Qt5Core.dll)
find_install_lib(Qt5Widgets.dll)
find_install_lib(Qt5Gui.dll)
find_install_lib(Qt5Multimedia.dll)
find_install_lib(Qt5MultimediaWidgets.dll)
find_install_lib(Qt5Network.dll)
find_install_lib(Qt5PrintSupport.dll)
find_install_lib(Qt5SVG.dll)
find_install_lib(Qt5Positioning.dll)
find_install_lib(Qt5Qml.dll)
find_install_lib(Qt5Quick.dll)
find_install_lib(Qt5Sensors.dll)
find_install_lib(Qt5WebKit.dll)
find_install_lib(Qt5WebChannel.dll)
find_install_lib(Qt5QmlModels.dll)
find_install_lib(Qt5WebKitWidgets.dll)
find_install_lib(libpcre2-16-0.dll)
find_install_lib(zlib1.dll)
find_install_lib(libzstd.dll)
find_install_lib(libpng16-16.dll)
find_install_lib(libharfbuzz-0.dll)
find_install_lib(libglib-2.0-0.dll)
find_install_lib(libfreetype-6.dll)
find_install_lib(libsqlite3-0.dll)
find_install_lib(libwebp-7.dll)
find_install_lib(libjpeg-8.dll)
find_install_lib(libwoff2dec.dll)
find_install_lib(libwoff2common.dll)
find_install_lib(libxslt-1.dll)
find_install_lib(libxml2-2.dll)
find_install_lib(libgraphite2.dll)
find_install_lib(libbrotlidec.dll)
find_install_lib(libbz2-1.dll)
find_install_lib(libintl-8.dll)
find_install_lib(libpcre-1.dll)
find_install_lib(libiconv-2.dll)

find_install_lib(libicudt68.dll)
find_install_lib(libicuin68.dll)
find_install_lib(libicuuc68.dll)
find_install_lib(libdouble-conversion.dll)
find_install_lib(libwinpthread-1.dll)
find_install_lib(libstdc++-6.dll)
find_install_lib(libgcc_s_seh-1.dll)

set(CMAKE_INSTALL_SYSTEM_RUNTIME_COMPONENT Runtime)
set(CMAKE_INSTALL_SYSTEM_RUNTIME_DESTINATION ${CMAKE_INSTALL_BINDIR})
set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS_NO_WARNINGS FALSE)
if(DEBUG)
  set(CMAKE_INSTALL_DEBUG_LIBRARIES TRUE)
endif()
#</InstallRequiredSystemLibraries>

include(InstallRequiredSystemLibraries)
include(CPack)