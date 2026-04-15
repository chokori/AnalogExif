cmake_minimum_required(VERSION 3.21)
set(X_VCPKG_APPLOCAL_DEPS_INSTALL ON)
project(AnalogExif LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_AUTOUIC_SEARCH_PATHS "ui")
set(CMAKE_AUTORCC_SEARCH_PATHS "res")

# Qt 6.11 modules
find_package(Qt6 6.11.0 REQUIRED COMPONENTS
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



if(WIN32)
    get_target_property(_qmake_executable Qt6::qmake IMPORTED_LOCATION)
    get_filename_component(_qt_bin_dir "${_qmake_executable}" DIRECTORY)
    find_program(WINDEPLOYQT_EXECUTABLE windeployqt HINTS "${_qt_bin_dir}")

    add_custom_command(TARGET AnalogExif POST_BUILD
        COMMAND "${WINDEPLOYQT_EXECUTABLE}"
                --no-compiler-runtime
                --no-svg
                --no-translations
                --verbose 0
                \"$<TARGET_FILE:AnalogExif>\"
    )

    install(TARGETS AnalogExif RUNTIME DESTINATION .)
    install(CODE "
        execute_process(COMMAND \"${WINDEPLOYQT_EXECUTABLE}\" 
            --no-compiler-runtime 
            --no-svg 
            --no-translations 
            --verbose 0 
            \"\${CMAKE_INSTALL_PREFIX}/AnalogExif.exe\") 
    ")
	
    install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/res/db/AnalogExif.ael" DESTINATION "db")

	set(CPACK_NSIS_EXTRA_INSTALL_COMMANDS "
        SetShellVarContext current
        IfFileExists '$DOCUMENTS/AnalogExif/AnalogExif.ael' db_exists 0 
            CreateDirectory '$DOCUMENTS/AnalogExif'
            CopyFiles /SILENT '$INSTDIR/db/AnalogExif.ael' '$DOCUMENTS/AnalogExif/AnalogExif.ael'
            Goto copy_done
        db_exists:
            DetailPrint 'Database file already exists. Skipping copy.'
        copy_done:
        RMDir /r '$INSTDIR/db'
    ")
	
    set(CPACK_NSIS_EXTRA_UNINSTALL_COMMANDS "
        SetShellVarContext current
        Delete '$DESKTOP/AnalogExif.lnk'
        MessageBox MB_YESNO|MB_ICONQUESTION 'Delete the database file in your Documents?' IDNO skip_delete
        Delete '$DOCUMENTS/AnalogExif/AnalogExif.ael'
        RMDir '$DOCUMENTS/AnalogExif'
        skip_delete:
    ")

    set(CPACK_PACKAGE_NAME "AnalogExif")
    set(CPACK_PACKAGE_VENDOR "chokori")
    set(CPACK_PACKAGE_VERSION "1.0.0")
    set(CPACK_GENERATOR "NSIS")
    set(CPACK_PACKAGE_INSTALL_DIRECTORY "AnalogExif")
    set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
    set(CPACK_NSIS_CREATE_ICONS_EXTRA "CreateShortCut '$DESKTOP/AnalogExif.lnk' '$INSTDIR/AnalogExif.exe'")
    
    include(CPack)
endif()