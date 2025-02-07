cmake_minimum_required(VERSION 3.10)

project(ntv2)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

# AJA includes
include(AJAHelpers)
include(AJABuildOptions)
include(AJACommonDefines)
include(AJACommonFlags)
include(AJACommonPaths)
include(AJACommonVars)
include(AJAVersionConfig)
# CMake includes
include(GNUInstallDirs)

aja_message(STATUS "Platform: ${CMAKE_SYSTEM_NAME}")
aja_message(STATUS "Arch: ${CMAKE_HOST_SYSTEM_PROCESSOR}")
aja_message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID}")
aja_message(STATUS "Install Prefix: ${CMAKE_INSTALL_PREFIX}")
if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    aja_message(STATUS "Windows API Version: ${CMAKE_SYSTEM_VERSION}")
    string(REPLACE "." ";" AJA_WIN_API_VER "${CMAKE_SYSTEM_VERSION}")
    list(GET AJA_WIN_API_VER 0 AJA_WIN_API_VER_MAJOR)
    list(GET AJA_WIN_API_VER 1 AJA_WIN_API_VER_MINOR)
    list(GET AJA_WIN_API_VER 2 AJA_WIN_API_VER_BUILD)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    aja_message(STATUS "macOS Architectures: ${CMAKE_OSX_ARCHITECTURES}")
    aja_message(STATUS "macOS Deployment Target: ${CMAKE_OSX_DEPLOYMENT_TARGET}")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
else()
    aja_message(FATAL_ERROR "Unsupported platform: ${CMAKE_SYSTEM_NAME}")
endif()

# Add NTV2 sub-directories to CMake build config
if (AJA_BUILD_APPS AND EXISTS ${AJA_APPS_ROOT})
    aja_message(STATUS "adding ajaapps...")
    add_subdirectory(ajaapps)
endif()
if (AJA_BUILD_DRIVER AND EXISTS ${AJA_DRIVER_ROOT})
    aja_message(STATUS "adding ajadriver...")
    add_subdirectory(ajadriver)
endif()
if (AJA_BUILD_LIBS AND EXISTS ${AJA_LIBRARIES_ROOT})
    aja_message(STATUS "adding ajalibraries...")
    add_subdirectory(ajalibraries)
endif()
if (AJA_BUILD_PLUGINS AND EXISTS ${AJA_PLUGINS_ROOT})
    aja_message(STATUS "adding ajaplugins...")
    add_subdirectory(ajaplugins)
endif()
if (AJA_BUILD_QA AND EXISTS ${AJA_QA_ROOT})
    aja_message(STATUS "adding qa...")
    add_subdirectory(qa)
endif()
if (AJA_BUILD_NONAJA AND EXISTS ${NON_AJA_ROOT})
    aja_message(STATUS "adding non-aja...")
    add_subdirectory(non-aja)
endif()

# Install NTV2 CMake files
if (AJA_INSTALL_SOURCES)
    install(FILES cmake/AJABuildOptions.cmake                DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJACommonDefines.cmake               DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJACommonFlags.cmake                 DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJACommonPaths.cmake                 DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJACommonVars.cmake                  DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJAHelpers.cmake                     DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJAQtHelpers.cmake                   DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/AJAVersionConfig.cmake               DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/linux_ldd_qt_libs.cmake              DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake)
    install(FILES cmake/toolchain/aja-ci-toolchain.cmake     DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/toolchain)
    install(FILES cmake/toolchain/clang-unix_toolchain.cmake DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/toolchain)
    install(FILES cmake/toolchain/clang-win_toolchain.cmake  DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/toolchain)
    install(FILES cmake/toolchain/msvc_toolchain.cmake       DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/toolchain)
    install(FILES cmake/bundle/linux/lin-ver.json.in         DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/bundle/linux)
    install(FILES cmake/bundle/mac/Info.plist.in             DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/bundle/mac)
    install(FILES cmake/bundle/windows/win-ver.rc.in         DESTINATION ${CMAKE_INSTALL_PREFIX}/cmake/bundle/windows)
    install(FILES CMakeLists.txt                             DESTINATION ${CMAKE_INSTALL_PREFIX}/)
endif()
