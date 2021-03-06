
include_directories( ./ ../  ../include  ../common)

QT5_ADD_RESOURCES (RES_SRC  resource.qrc)

QT5_WRAP_UI ( UI_SRC
    mainwindow.ui
    removecurvedialog.ui
    busydialog.ui
    curvecolorpick.ui
    filterablelistwidget.ui
    tabbedplotwidget.ui
    ../common/selectlistdialog.ui
    )

SET( PLOTTER_SRC
    main.cpp
    mainwindow.cpp
    plotwidget.cpp
    plotmatrix.cpp
    removecurvedialog.cpp
    plotmagnifier.cpp
    busydialog.cpp
    busytaskdialog.cpp
    customtracker.cpp
    curvecolorpick.cpp
    plotdata_qwt.cpp
    filterablelistwidget.cpp
    tabbedplotwidget.cpp
    subwindow.cpp

    mainwindow.h
    ../common/selectlistdialog.h
    ../include/PlotJuggler/plotdata.h
    )

add_executable(PlotJuggler ${PLOTTER_SRC} ${RES_SRC} ${UI_SRC})


target_link_libraries(PlotJuggler
    ${Qt5Core_QTMAIN_LIBRARIES}
    ${Qt5Widgets_LIBRARIES}
    ${Qt5PrintSupport_LIBRARIES}
    ${Qt5Xml_LIBRARIES}
    ${Qt5Concurrent_LIBRARIES}
    colorwidgets
    qwt_static
    )

if(catkin_FOUND)
    install(TARGETS PlotJuggler
       ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
     )
elseif()
    install(TARGETS PlotJuggler DESTINATION ${CMAKE_INSTALL_PREFIX}/bin  )
endif()
