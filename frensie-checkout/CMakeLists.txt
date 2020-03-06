# Configure the Dockerfile
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/Dockerfile.in ${CMAKE_CURRENT_BINARY_DIR}/Dockerfile @ONLY)

# Configure the frensie build script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/frensie.sh.in ${CMAKE_CURRENT_BINARY_DIR}/frensie.sh @ONLY)

# Configure the environment shell script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/env.sh.in ${CMAKE_CURRENT_BINARY_DIR}/env.sh @ONLY)

# Configure the build script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/build-frensie-checkout.sh.in ${CMAKE_CURRENT_BINARY_DIR}/build-frensie-checkout.sh @ONLY)

# Configure the run script
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/run-frensie-checkout.sh.in ${CMAKE_CURRENT_BINARY_DIR}/run-frensie-checkout.sh @ONLY)

ADD_CUSTOM_COMMAND(OUTPUT ${FRENSIE_CHECKOUT_SUCCESS_FILE}
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMAND ./build-frensie-checkout.sh)

ADD_CUSTOM_TARGET(${FRENSIE_CHECKOUT_TARGET} ALL
  DEPENDS ${FRENSIE_CHECKOUT_SUCCESS_FILE} ${BUILD_DEPS_TARGET} ${DATA_DIR_LINK})
