
include_directories( ./ ../  ../../include  ../../common)

add_definitions(${QT_DEFINITIONS})
add_definitions(-DQT_PLUGIN)
add_definitions(-DQT_SHARED)


#QT5_WRAP_UI ( UI_SRC  ../common/selectlistdialog.ui  )


SET( SRC
    datastream_sample.cpp
    ../../include/PlotJuggler/datastreamer_base.h
    )

add_library(DataStreamSample SHARED ${SRC} ${UI_SRC}  )


target_link_libraries(DataStreamSample  ${Qt5Widgets_LIBRARIES} )

if(catkin_FOUND)
    install(TARGETS DataStreamSample
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION} )
elseif()
    install(TARGETS DataStreamSample DESTINATION ${CMAKE_INSTALL_PREFIX}/bin  )
endif()
