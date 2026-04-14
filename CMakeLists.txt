cmake_minimum_required(VERSION 3.21)
project(AnalogExif LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_AUTOUIC_SEARCH_PATHS "ui")
set(CMAKE_AUTORCC_SEARCH_PATHS "res")

# Qt 6.11 modules
find_package(Qt6 REQUIRED COMPONENTS
    Widgets
    Network
    Sql
    Concurrent
)

# Exiv2 + Expat (vcpkg)
find_package(exiv2 0.28.8 REQUIRED)
find_package(EXPAT 2.7.4 REQUIRED)

# Sources
set(SOURCES
    src/aboutdialog.cpp
    src/analogexif.cpp
    src/analogexifoptions.cpp
    src/autofillexpnum.cpp
    src/copymetadatadialog.cpp
    src/dirsortfilterproxymodel.cpp
    src/editgear.cpp
    src/editgeartagsmodel.cpp
    src/editgeartreemodel.cpp
    src/edittagselectvalues.cpp
    src/exifitem.cpp
    src/exifitemdelegate.cpp
    src/exiftreemodel.cpp
    src/exifutils.cpp
    src/gearlistmodel.cpp
    src/geartreemodel.cpp
    src/main.cpp
    src/metadatatagcompleter.cpp
    src/multitagvaluesdialog.cpp
    src/onlineversionchecker.cpp
    src/optgeartemplatemodel.cpp
    src/progressdialog.cpp
    src/tagnameitemdelegate.cpp
    src/tagselectvalsitemdelegate.cpp
    src/tagtypeitemdelegate.cpp
    src/asciitextdialog.cpp
    src/asciistringdialog.cpp
    src/tagnameeditdialog.cpp
)

set(HEADERS
    include/aboutdialog.h
    include/analogexif.h
    include/analogexifoptions.h
    include/autofillexpnum.h
    include/checkedgeartreeview.h
    include/copymetadatadialog.h
    include/dirsortfilterproxymodel.h
    include/editgear.h
    include/editgeartagsmodel.h
    include/editgeartreemodel.h
    include/edittagselectvalues.h
    include/emptyspinbox.h
    include/exifitem.h
    include/exifitemdelegate.h
    include/exiftreemodel.h
    include/exifutils.h
    include/gearlistmodel.h
    include/gearlistview.h
    include/geartableview.h
    include/geartreemodel.h
    include/geartreeview.h
    include/metadatatagcompleter.h
    include/multitagvaluesdialog.h
    include/onlineversionchecker.h
    include/optgeartemplatemodel.h
    include/optgeartemplateview.h
    include/progressdialog.h
    include/tagnameitemdelegate.h
    include/tagselectvalsitemdelegate.h
    include/tagtypeitemdelegate.h
    include/asciitextdialog.h
    include/asciistringdialog.h
    include/tagnameeditdialog.h
)

set(UI_FILES
    ui/analogexifoptions.ui
    ui/autofillexpnum.ui
    ui/copymetadatadialog.ui
    ui/edittagselectvalues.ui
    ui/multitagvaluesdialog.ui
    ui/progressdialog.ui
    ui/tagnameeditdialog.ui
    ui/aboutdialog.ui
    ui/analogexif.ui
    ui/editgear.ui
    ui/asciistringdialog.ui
    ui/asciitextdialog.ui
)

set(RESOURCES
   res/analogexif.qrc
   res/AnalogEXif.rc
)

qt_add_executable(AnalogExif WIN32
    ${SOURCES}
    ${HEADERS}
    ${UI_FILES}
    ${RESOURCES}
)

target_include_directories(AnalogExif PRIVATE include)

target_link_libraries(AnalogExif PRIVATE
    Qt6::Widgets
    Qt6::Network
    Qt6::Sql
    Qt6::Concurrent
    exiv2lib
    EXPAT::EXPAT
)

# macOS icon
if(APPLE)
    set_source_files_properties(res/AnalogExif.icns PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")
    target_sources(AnalogExif PRIVATE res/AnalogExif.icns)
    set_target_properties(AnalogExif PROPERTIES
    MACOSX_BUNDLE_ICON_FILE AnalogExif.icns
    MACOSX_BUNDLE TRUE)
endif()
