
add_library(gz-sim-yarp-device-registry SHARED DeviceRegistry.hh DeviceRegistry.cc)
add_library(${PROJECT_NAME}::gz-sim-yarp-device-registry ALIAS gz-sim-yarp-device-registry)

target_include_directories(gz-sim-yarp-device-registry PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
                                                              "$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>")

target_compile_features(gz-sim-yarp-device-registry PRIVATE cxx_std_17)

target_link_libraries(gz-sim-yarp-device-registry
        PRIVATE gz-plugin${GZ_PLUGIN_VER}::gz-plugin${GZ_PLUGIN_VER}
        PUBLIC gz-sim${GZ_SIM_VER}::gz-sim${GZ_SIM_VER}
        PRIVATE YARP::YARP_os
        PRIVATE YARP::YARP_init
        PUBLIC  YARP::YARP_dev)

install(TARGETS gz-sim-yarp-device-registry
        EXPORT ${PROJECT_NAME})

install(FILES DeviceRegistry.hh
        DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/gzyarp")
