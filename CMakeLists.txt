cmake_minimum_required(VERSION 3.20)
set(CMAKE_SIZEOF_VOID_P 8) # 64-bit baybeeeeeeeee
project(SUBSTitute
        VERSION 1.1.6
        DESCRIPTION "A GUI wrapper around the drive mapping tool subst"
        LANGUAGES NONE)

if(NOT WIN32)
    message(FATAL_ERROR "Windows Only")
endif()

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

# We don't want to have to re-structure the whole project again in cmake when there's a vs solution right here
# Custom command to invoke dotnet
set(publish_path ${CMAKE_CURRENT_LIST_DIR}/bin/$<CONFIG>/net5.0-windows/publish)
set(build_product ${publish_path}/SUBSTitute.exe)

add_custom_command(OUTPUT ${build_product}
                   COMMAND dotnet publish SUBSTitute.sln -c $<CONFIG>
                   WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR})

add_custom_target(SUBSTitute ALL DEPENDS ${build_product})

install(DIRECTORY ${publish_path}/ DESTINATION .)

find_package(WIX REQUIRED)

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
set(CPACK_PACKAGE_EXECUTABLES "SUBSTitute" SUBSTitute.exe)

set(CPACK_WIX_UPGRADE_GUID "38F2034A-2460-4DF6-A786-E65D7F24D27D")

include (CPack)
