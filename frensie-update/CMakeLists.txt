# Configure the Dockerfile
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/Dockerfile.in ${CMAKE_CURRENT_BINARY_DIR}/Dockerfile @ONLY)

# Configure the frensie environment script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/frensie-env.sh.in ${CMAKE_CURRENT_BINARY_DIR}/frensie-env.sh @ONLY)

# Configure the build script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/build-frensie-update.sh.in ${CMAKE_CURRENT_BINARY_DIR}/build-frensie-update.sh @ONLY)

# Configure the run script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/run-frensie-update.sh.in ${CMAKE_CURRENT_BINARY_DIR}/run-frensie-update.sh @ONLY)

ADD_CUSTOM_COMMAND(OUTPUT ${FRENSIE_UPDATE_SUCCESS_FILE}
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMAND ./build-frensie-update.sh)

ADD_CUSTOM_TARGET(${FRENSIE_UPDATE_TARGET} ALL
  DEPENDS ${FRENSIE_UPDATE_SUCCESS_FILE} ${FRENSIE_CHECKOUT_TARGET})
