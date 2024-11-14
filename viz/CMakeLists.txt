rock_find_qt4(OPTIONAL)
rock_find_qt5(OPTIONAL Widgets)
find_package(PkgConfig REQUIRED)

# NOTE: Ubuntu versions use different naming conventions for sdformat packages.
set(SDFORMAT_VERSIONS
    "sdformat"
    "sdformat9"
    "sdformat6"
    "sdformat12"
)
set(SDFORMAT_FOUND false)
foreach(version ${SDFORMAT_VERSIONS})
    pkg_check_modules(SDFORMAT_${version} QUIET ${version})
    if(SDFORMAT_${version}_FOUND)
        set(SDFORMAT_PKGCONFIG_NAME ${version})
        set(SDFORMAT_FOUND true)
        set(CMAKE_CXX_STANDARD 17)
        set(CMAKE_CXX_STANDARD_REQUIRED ON)
        message(STATUS "Found ${version} using pkg-config")
        break()
    endif()
endforeach()
if(NOT SDFORMAT_FOUND)
    message(SEND_ERROR "Could not find any version of sdformat with pkg-config")
endif()

if (ROCK_QT_VERSION_4)
	if (vizkit3d_FOUND)
		rock_vizkit_plugin(robot_model-viz
        RobotVisualization.cpp RobotModel.cpp OSGHelpers.hpp OSGSegment.cpp VBOVisitor.hpp
        DEPS_PKGCONFIG kdl_parser base-types base-viz base-lib base-logging sdformat urdfdom
        MOC RobotVisualization.hpp
        HEADERS RobotVisualization.hpp RobotModel.h OSGSegment.h VBOVisitor.hpp)
	else()  
		message(STATUS "vizkit3d not found ... NOT building the robot_model-viz plugins")
	endif()
endif()
if (ROCK_QT_VERSION_5)
    if (vizkit3d-qt5_FOUND)
        rock_vizkit_plugin_qt5(robot_model-viz-qt5
        RobotVisualization.cpp RobotModel.cpp OSGHelpers.hpp OSGSegment.cpp VBOVisitor.hpp
        DEPS_PKGCONFIG kdl_parser base-types base-viz-qt5 base-lib base-logging ${SDFORMAT_PKGCONFIG_NAME} urdfdom
        MOC5 RobotVisualization.hpp
        HEADERS RobotVisualization.hpp RobotModel.h OSGSegment.h VBOVisitor.hpp)
	else()  
		message(STATUS "vizkit3d-qt5 not found ... NOT building the robot_model-viz-qt5 plugins")
	endif()
endif()
