cmake_minimum_required (VERSION 3.14)
project(win-manifests VERSION 0.0.1)

add_library(win-manifests INTERFACE)

if(WIN32)
target_sources(win-manifests INTERFACE
"$<INSTALL_INTERFACE:share/win-manifests/utf-8.manifest>"
"$<INSTALL_INTERFACE:share/win-manifests/heapType.manifest>"
"$<INSTALL_INTERFACE:share/win-manifests/dpiAwareness.manifest>"
"$<INSTALL_INTERFACE:share/win-manifests/osVersion.manifest>"
"$<INSTALL_INTERFACE:share/win-manifests/longPathAwareness.manifest>"
# "$<INSTALL_INTERFACE:share/win-manifests/opt-trustinfo.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/utf-8.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/heapType.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/dpiAwareness.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/osVersion.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/longPathAwareness.manifest>"
# "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/opt-trustinfo.manifest>"
)
endif()

if(${CMAKE_PROJECT_NAME} STREQUAL ${PROJECT_NAME})
include(CPack)
include(GNUInstallDirs)
install(TARGETS win-manifests
    EXPORT  win-manifests
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
install(EXPORT  win-manifests
    FILE win-manifestsConfig.cmake
    NAMESPACE win-manifests::
    DESTINATION share/win-manifests
)

if(WIN32)
file(GLOB MANIFESTS "${CMAKE_CURRENT_SOURCE_DIR}/*.manifest")
install(FILES ${MANIFESTS}
    DESTINATION share/win-manifests
)
endif()

endif()
