add_library(CesiumIonClient "")

configure_cesium_library(CesiumIonClient)

cesium_glob_files(CESIUM_ION_CLIENT_SOURCES src/*.cpp)
cesium_glob_files(CESIUM_ION_CLIENT_HEADERS src/*.h)
cesium_glob_files(CESIUM_ION_CLIENT_PUBLIC_HEADERS include/CesiumIonClient/*.h)
cesium_glob_files(CESIUM_ION_CLIENT_TEST_SOURCES test/*.cpp)
cesium_glob_files(CESIUM_ION_CLIENT_TEST_HEADERS test/*.h)

set_target_properties(CesiumIonClient
    PROPERTIES
        TEST_SOURCES "${CESIUM_ION_CLIENT_TEST_SOURCES}"
        TEST_HEADERS "${CESIUM_ION_CLIENT_TEST_HEADERS}"
)

set_target_properties(CesiumIonClient
    PROPERTIES
    PUBLIC_HEADER "${CESIUM_ION_CLIENT_PUBLIC_HEADERS}"
)

target_sources(
    CesiumIonClient
    PRIVATE
        ${CESIUM_ION_CLIENT_SOURCES}
        ${CESIUM_ION_CLIENT_HEADERS}
        ${CESIUM_ION_CLIENT_PUBLIC_HEADERS}
)

target_include_directories(
    CesiumIonClient
    SYSTEM PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_link_libraries(CesiumIonClient
    PUBLIC
        CesiumAsync
        CesiumUtility
    PRIVATE
        httplib::httplib
        modp_b64
        PicoSHA2
)

if (NOT ${CMAKE_SYSTEM_NAME} MATCHES "WindowsStore")
  target_link_libraries(CesiumIonClient PRIVATE csprng)
endif()

install(TARGETS CesiumIonClient
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/CesiumIonClient
)
