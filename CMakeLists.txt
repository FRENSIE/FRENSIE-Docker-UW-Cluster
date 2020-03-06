# Define the minimum required version of CMake
CMAKE_MINIMUM_REQUIRED(VERSION 3.0.1 FATAL_ERROR)

# Add the custom modules to the module path
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
  ${CMAKE_SOURCE_DIR}/cmake)

# Define the project name
SET(PROJECT_NAME FRENSIE-Docker-UW-Cluster)
PROJECT(${PROJECT_NAME} NONE)

# Define the project version
SET(${PROJECT_NAME}_VERSION 1.0.0)
SET(${PROJECT_NAME}_MAJOR_VERSION 1)
SET(${PROJECT_NAME}_MAJOR_MINOR_VERSION 1000000)
SET(${PROJECT_NAME}_VERSION_STRING "1.0.0")

# Include the macros that have been defined for this project
INCLUDE(Macros)

# Enforce an out-of-source-build
ENFORCE_OUT_OF_SOURCE_BUILD()

# Add an uninstall target
ADD_UNINSTALL_TARGET()

# Check that the user name on the UW cluster has been specified
IF(NOT DEFINED UW_CLUSTER_USER_NAME)
  MESSAGE(FATAL_ERROR "The user name on the UW cluster must be specified!")
ENDIF()

# Check that the required paths have been specified
IF(NOT DEFINED UW_CLUSTER_FRENSIE_GROUP_SOFTWARE_DIR)
  MESSAGE(FATAL_ERROR "The FRENSIE group software directory location on the UW cluster must be specified!")
ENDIF()

IF(NOT DEFINED UW_CLUSTER_USER_DIR)
  MESSAGE(FATAL_ERROR "The user directory location on the UW cluster must be specified!")
ENDIF()

# Determine the frensie checkout directory
STRING(TOLOWER ${CMAKE_BUILD_TYPE} CMAKE_BUILD_TYPE)

IF(${CMAKE_BUILD_TYPE} MATCHES "release")
  SET(FRENSIE_CHECKOUT_DIR "frensie_rel" CACHE STRING "The FRENSIE checkout directory")
ELSEIF(${CMAKE_BUILD_TYPE} MATCHES "debug")
  SET(FRENSIE_CHECKOUT_DIR "frensie_db" CACHE STRING "The FRENSIE checkout directory")
ELSE()
  MESSAGE(FATAL_ERROR "The requested build type is not valid!")
ENDIF()

# Set up the project directory hierarchy
SET(BASE_IMAGE_TAG base)
SET(BUILD_DEPS_IMAGE_TAG deps)
SET(FRENSIE_CHECKOUT_IMAGE_TAG frensie-checkout)
SET(FRENSIE_UPDATE_IMAGE_TAG frensie-update)

SET(BASE_SUCCESS_FILE base-built.txt)
SET(BUILD_DEPS_SUCCESS_FILE build-deps-built.txt)
SET(FRENSIE_CHECKOUT_SUCCESS_FILE frensie-checkout-built.txt)
SET(FRENSIE_UPDATE_SUCCESS_FILE frensie-update-built.txt)

SET(BASE_TARGET base_image)
SET(BUILD_DEPS_TARGET deps_image)
SET(FRENSIE_CHECKOUT_TARGET frensie_checkout_image)
SET(FRENSIE_UPDATE_TARGET frensie_update_image)

ADD_SUBDIRECTORY(base)
ADD_SUBDIRECTORY(build-deps)
ADD_SUBDIRECTORY(frensie-checkout)
ADD_SUBDIRECTORY(frensie-update)

CONFIGURE_FILE(${CMAKE_SOURCE_DIR}/scripts/clean-docker.sh.in
  ${CMAKE_CURRENT_BINARY_DIR}/clean-docker.sh @ONLY)

ADD_CUSTOM_TARGET(clean-docker
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMAND ./clean-docker.sh)

ADD_CUSTOM_TARGET(clean-docker-update
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMAND ./clean-docker.sh -u)
