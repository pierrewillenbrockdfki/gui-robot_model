rock_find_qt4(OPTIONAL)
rock_find_qt5(OPTIONAL Widgets)

pkg_check_modules(SDFORMAT QUIET sdformat)
pkg_check_modules(SDFORMAT9 QUIET sdformat9)
pkg_check_modules(SDFORMAT12 QUIET sdformat12)
pkg_check_modules(SDFORMAT15 QUIET sdformat15)

if(${SDFORMAT_FOUND})
  set(SDFORMAT_PKGCONFIG_NAME "sdformat")
elseif(${SDFORMAT9_FOUND})
  set(SDFORMAT_PKGCONFIG_NAME "sdformat9")
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
elseif(${SDFORMAT12_FOUND})
  set(SDFORMAT_PKGCONFIG_NAME "sdformat12")
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
elseif(${SDFORMAT15_FOUND})
  set(SDFORMAT_PKGCONFIG_NAME "sdformat15")
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
else()
  message(SEND_ERROR "Could not find sdformat, sdformat9, sdformat12 or sdformat15 with pkg-config")
endif()


if(ROCK_QT_VERSION_4)
    rock_vizkit_plugin(robot_model-viz
        RobotVisualization.cpp RobotModel.cpp OSGHelpers.hpp OSGSegment.cpp VBOVisitor.hpp
        DEPS_PKGCONFIG kdl_parser base-types base-viz base-lib base-logging ${SDFORMAT_PKGCONFIG_NAME} urdfdom
        MOC RobotVisualization.hpp
        HEADERS RobotVisualization.hpp RobotModel.h OSGSegment.h VBOVisitor.hpp)
endif(ROCK_QT_VERSION_4)

if(ROCK_QT_VERSION_5)
    rock_vizkit_plugin_qt5(robot_model-viz-qt5
        RobotVisualization.cpp RobotModel.cpp OSGHelpers.hpp OSGSegment.cpp VBOVisitor.hpp
        DEPS_PKGCONFIG kdl_parser base-types base-viz-qt5 base-lib base-logging ${SDFORMAT_PKGCONFIG_NAME} urdfdom
        MOC5 RobotVisualization.hpp
        HEADERS RobotVisualization.hpp RobotModel.h OSGSegment.h VBOVisitor.hpp)
endif(ROCK_QT_VERSION_5)
