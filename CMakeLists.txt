cmake_minimum_required(VERSION 2.8.9)

project(DiagnosticIndexExtension)
set(LOCAL_PROJECT_NAME DiagnosticIndexExtension)

#-----------------------------------------------------------------------------
# Update CMake module path
#------------------------------------------------------------------------------
set(CMAKE_MODULE_PATH
   ${CMAKE_SOURCE_DIR}/CMake
   ${CMAKE_SOURCE_DIR}/SuperBuild
   ${CMAKE_MODULE_PATH}
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

#-----------------------------------------------------------------------------
# CMake Function(s) and Macro(s)
#-----------------------------------------------------------------------------
if(CMAKE_VERSION VERSION_LESS 2.8.3)
  include(Pre283CMakeParseArguments)
else()
  include(CMakeParseArguments)
endif()

#-----------------------------------------------------------------------------
# Set a default build type if none was specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

#-----------------------------------------------------------------------------
# Extension meta-information
set(EXTENSION_NAME "DiagnosticIndex")
set(EXTENSION_HOMEPAGE "http://www.slicer.org/slicerWiki/index.php/Documentation/Nightly/Extensions/DiagnosticIndex")
set(EXTENSION_CATEGORY "Quantification")
set(EXTENSION_CONTRIBUTORS "Laura Pascal (DCBIA-OrthoLab, University of Michigan)")
set(EXTENSION_DESCRIPTION "DiagnosticIndex is used to define the OA type of a patient according a Classification Groups that you can create.")
set(EXTENSION_ICONURL "TODO")
set(EXTENSION_SCREENSHOTURLS "TODO")
set(EXTENSION_DEPENDS ShapePopulationViewer)
set(EXTENSION_BUILD_SUBDIRECTORY ${LOCAL_PROJECT_NAME}-build)

#-----------------------------------------------------------------------------
include(ExternalProject)

#-----------------------------------------------------------------------------
# Extension dependencies

# Slicer
find_package(Slicer REQUIRED)
include(${Slicer_USE_FILE})
include(SlicerExtensionsConfigureMacros)
#resetForSlicer( NAMES CMAKE_C_COMPILER CMAKE_CXX_COMPILER CMAKE_CXX_FLAGS CMAKE_C_FLAGS )

# Slicer Execution Model
find_package(SlicerExecutionModel REQUIRED)
include(${SlicerExecutionModel_USE_FILE})
include(${GenerateCLP_USE_FILE})

#-----------------------------------------------------------------------------
option(${LOCAL_PROJECT_NAME}_SUPERBUILD "Build ${LOCAL_PROJECT_NAME} and the projects it depends on via SuperBuild.cmake." ON)

if(${${LOCAL_PROJECT_NAME}_SUPERBUILD})
	#-----------------------------------------------------------------------------
	include(${CMAKE_CURRENT_SOURCE_DIR}/SuperBuild.cmake)
else()
	include(${CMAKE_CURRENT_SOURCE_DIR}/DiagnosticIndex.cmake)
endif()


#-----------------------------------------------------------------------------
set(CPACK_INSTALL_CMAKE_PROJECTS "${CPACK_INSTALL_CMAKE_PROJECTS};${CMAKE_BINARY_DIR};${EXTENSION_NAME};ALL;/")
include(${Slicer_EXTENSION_CPACK})