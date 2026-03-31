cmake_minimum_required(VERSION 3.21)
project(AnalogExif LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# Qt 6.11 modules
find_package(Qt6 6.11 REQUIRED COMPONENTS
    Widgets
    Network
    Sql
    Concurrent
)

# Qt SQLite plugin (built-in)
find_package(Qt6 REQUIRED COMPONENTS Sql)

# Exiv2 + Expat (vcpkg)
find_package(exiv2 REQUIRED)
find_package(EXPAT REQUIRED)

# Sources
set(SOURCES
    aboutdialog.cpp
    analogexif.cpp
    analogexifoptions.cpp
    autofillexpnum.cpp
    copymetadatadialog.cpp
    dirsortfilterproxymodel.cpp
    editgear.cpp
    editgeartagsmodel.cpp
    editgeartreemodel.cpp
    edittagselectvalues.cpp
    exifitem.cpp
    exifitemdelegate.cpp
    exiftreemodel.cpp
    exifutils.cpp
    gearlistmodel.cpp
    geartreemodel.cpp
    main.cpp
    metadatatagcompleter.cpp
    multitagvaluesdialog.cpp
    onlineversionchecker.cpp
    optgeartemplatemodel.cpp
    progressdialog.cpp
    tagnameitemdelegate.cpp
    tagselectvalsitemdelegate.cpp
    tagtypeitemdelegate.cpp
    asciitextdialog.cpp
    asciistringdialog.cpp
    tagnameeditdialog.cpp
)

set(HEADERS
    aboutdialog.h
    analogexif.h
    analogexifoptions.h
    autofillexpnum.h
    checkedgeartreeview.h
    copymetadatadialog.h
    dirsortfilterproxymodel.h
    editgear.h
    editgeartagsmodel.h
    editgeartreemodel.h
    edittagselectvalues.h
    emptyspinbox.h
    exifitem.h
    exifitemdelegate.h
    exiftreemodel.h
    exifutils.h
    gearlistmodel.h
    gearlistview.h
    geartableview.h
    geartreemodel.h
    geartreeview.h
    metadatatagcompleter.h
    multitagvaluesdialog.h
    onlineversionchecker.h
    optgeartemplatemodel.h
    optgeartemplateview.h
    progressdialog.h
    tagnameitemdelegate.h
    tagselectvalsitemdelegate.h
    tagtypeitemdelegate.h
    asciitextdialog.h
    asciistringdialog.h
    tagnameeditdialog.h
)

set(FORMS
    analogexifoptions.ui
    autofillexpnum.ui
    copymetadatadialog.ui
    edittagselectvalues.ui
    multitagvaluesdialog.ui
    progressdialog.ui
    tagnameeditdialog.ui
    aboutdialog.ui
    analogexif.ui
    editgear.ui
    asciistringdialog.ui
    asciitextdialog.ui
)

set(RESOURCES
    analogexif.qrc
)

add_executable(AnalogExif
    ${SOURCES}
    ${HEADERS}
    ${FORMS}
    ${RESOURCES}
)

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
    set_source_files_properties(AnalogExif.icns PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")
    target_sources(AnalogExif PRIVATE AnalogExif.icns)
endif()