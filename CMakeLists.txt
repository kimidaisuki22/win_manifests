cmake_minimum_required (VERSION 3.14)
project(win_manifests)

add_library(win_manifests INTERFACE)

if(WIN32)
target_sources(win_manifests INTERFACE
"$<INSTALL_INTERFACE:share/win_manifests/utf-8.manifest>"
"$<INSTALL_INTERFACE:share/win_manifests/heapType.manifest>"
"$<INSTALL_INTERFACE:share/win_manifests/dpiAwareness.manifest>"
"$<INSTALL_INTERFACE:share/win_manifests/osVersion.manifest>"
# "$<INSTALL_INTERFACE:share/win_manifests/opt-trustinfo.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/utf-8.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/heapType.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/dpiAwareness.manifest>"
"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/osVersion.manifest>"
# "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/opt-trustinfo.manifest>"
)
endif()

if(${CMAKE_PROJECT_NAME} STREQUAL ${PROJECT_NAME})
include(CPack)
include(GNUInstallDirs)
install(TARGETS win_manifests
    EXPORT  win_manifests
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
install(EXPORT  win_manifests
    FILE win_manifests.cmake
    NAMESPACE win_manifests::
    DESTINATION share/win_manifests
)

if(WIN32)
file(GLOB MANIFESTS "${CMAKE_CURRENT_SOURCE_DIR}/*.manifest")
install(FILES ${MANIFESTS}
    DESTINATION share/win_manifests
)
endif()

endif()
