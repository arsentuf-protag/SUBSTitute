cmake_minimum_required(VERSION 3.20)
project(SUBSTitute
        VERSION 1.1.4
        DESCRIPTION "A GUI wrapper around the drive mapping tool subst"
        LANGUAGES NONE)

if(NOT WIN32)
    message(FATAL_ERROR "Windows Only")
endif()

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(WIX REQUIRED)

set(publish_path ${CMAKE_CURRENT_LIST_DIR}/bin/Release/net5.0-windows/publish)

if(NOT EXISTS ${publish_path})
    message(FATAL_ERROR "Please publish a build with VS2019 first")
endif()

set(SUBSTitute SUBSTitute.exe)
file(GLOB files ${publish_path}/*.*)

install(FILES ${files} DESTINATION .)

set(CPACK_GENERATOR "WIX")
set(CPACK_PACKAGE_NAME "SUBSTitute")
set(CPACK_PACKAGE_DESCRIPTION "${CMAKE_PROJECT_DESCRIPTION}")
set(CPACK_PACKAGE_VERSION_MAJOR "${CMAKE_PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${CMAKE_PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${CMAKE_PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_VENDOR "David Sungaila")
set(CPACK_PACKAGE_ICON "${CMAKE_CURRENT_LIST_DIR}/SUBSTitute.ico")
set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${CMAKE_PROJECT_VERSION}")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_LIST_DIR}/licence.txt")
set(CPACK_PACKAGE_EXECUTABLES "SUBSTitute" ${SUBSTitute})

set(CPACK_WIX_UPGRADE_GUID "38F2034A-2460-4DF6-A786-E65D7F24D27D")

include (CPack)