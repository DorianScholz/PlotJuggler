project(plotjuggler)
cmake_minimum_required(VERSION 2.8)

find_package(Qt5 REQUIRED COMPONENTS Core Widgets PrintSupport Concurrent Xml)

if(catkin_FOUND)
    message("--------------------------------------------------------------------")
    message("PlotJuggler is being built using CATKIN. ROS plugin will be compiled")
    message("--------------------------------------------------------------------")

    find_package(catkin REQUIRED COMPONENTS
        rosbag
        rosbag_storage
        roscpp
        roscpp_serialization
        ros_type_introspection
        rostime
        topic_tools
        )
    catkin_package(
        INCLUDE_DIRS include
        CATKIN_DEPENDS ros_type_introspection
        DEPENDS system_lib
        )

endif()


set(CMAKE_AUTOMOC ON)
SET(CMAKE_AUTOUIC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fPIC")

include_directories(
    ${Qt5Widgets_INCLUDE_DIRS}
    ${Qt5Concurrent_INCLUDE_DIRS}
    ${Qt5PrintSupport_INCLUDE_DIRS}
    ${Qt5Xml_INCLUDE_DIRS}
    #${Qt5OpenGl_INCLUDE_DIRS}
    )

include_directories( ./qwt/src ./include ./color_widgets/include )


if(catkin_FOUND)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/${PROJECT_NAME})
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/${PROJECT_NAME})
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/${PROJECT_NAME})
elseif()
    set(CMAKE_INSTALL_PREFIX "/usr/local")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endif()


add_subdirectory( common)
add_subdirectory( color_widgets )
add_subdirectory( qwt/src )
add_subdirectory( plotter_gui )

add_subdirectory( plugins/DataLoadCSV )
add_subdirectory( plugins/DataStreamSample )

if(catkin_FOUND)
    add_subdirectory( plugins/ROS )
endif()


#########################  create a package ####################################

SET(CPACK_GENERATOR "TGZ;DEB")
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "Davide Faconti")
SET(CPACK_PACKAGE_CONTACT "faconti@icarustechnology.com")


SET(CPACK_PACKAGING_INSTALL_PREFIX "/usr/local/")
SET(CPACK_INCLUDE_TOPLEVEL_DIRECTORY 0)

SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "PlotJuggler: juggle with data")
SET(CPACK_PACKAGE_VENDOR "Icarus Technology")

SET(CPACK_PACKAGE_VERSION_MAJOR "0")
SET(CPACK_PACKAGE_VERSION_MINOR "6")
SET(CPACK_PACKAGE_VERSION_PATCH "0")
SET(CPACK_PACKAGE_VERSION "0.6.0")

SET(CPACK_PACKAGE_NAME "plotjuggler${CPACK_DEBIAN_PACKAGE_ARCHITECTURE}")
SET(CPACK_DEBIAN_PACKAGE_DEPENDS "qt5-default")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "PlotJuggler")

INCLUDE(CPack)
