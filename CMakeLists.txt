cmake_minimum_required (VERSION 2.6)

project(vsm-ardupilot)

# Deal with path to SDK
if (NOT DEFINED VSM_SDK_DIR)
    if (DEFINED ENV{VSM_SDK_DIR})
        set(VSM_SDK_DIR $ENV{VSM_SDK_DIR})
    else()
        # Default SDK path
        if(WIN32)
            set(VSM_SDK_DIR "C:/UGCS/vsm-sdk")
        elseif(APPLE)
            set(VSM_SDK_DIR "/Applications/UgCS/vsm-sdk")
        elseif(UNIX)
            set(VSM_SDK_DIR "/opt/ugcs/vsm-sdk")
        endif()
    endif()
endif()

set(UGCS_PACKAGE_SUMMARY "UgCS vehicle specific module (VSM) for vehicles driven by Ardupilot")
set(UGCS_VSM_USER_GUIDE_LATEX_TITLE "Ardupilot VSM User Guide")
set(CMAKE_MODULE_PATH "${VSM_SDK_DIR}/share/cmake")
include("ugcs/vsm")

Build_vsm_config(${CMAKE_SOURCE_DIR}/vsm.conf)

if (NOT DEFINED COMMON_SOURCES)
    set(COMMON_SOURCES "../common")
endif()

include_directories (include "${COMMON_SOURCES}/include")

file(GLOB SOURCES "src/*.cpp" "${COMMON_SOURCES}/src/*mavlink*.cpp") 
file(GLOB HEADERS "include/*.h" "${COMMON_SOURCES}/include/*mavlink*.h") 

Build_vsm()
