cmake_minimum_required(VERSION 2.8.12.2)
set (CMAKE_CXX_STANDARD 11)
project (sleepSync)
include_directories(${CMAKE_JS_INC})


# Selective Compile
if (WIN32)
    file(GLOB SOURCE_FILES "./src/sleep-sync-win.cpp")
endif (WIN32)
if (UNIX)
    file(GLOB SOURCE_FILES "./src/sleep-sync-unix.cpp")
endif (UNIX)


add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES} ${CMAKE_JS_SRC})
set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "" SUFFIX ".node")
target_link_libraries(${PROJECT_NAME} ${CMAKE_JS_LIB})
execute_process(COMMAND node -p "require('node-addon-api').include"
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        OUTPUT_VARIABLE NODE_ADDON_API_DIR
)
string(REPLACE "\n" "" NODE_ADDON_API_DIR ${NODE_ADDON_API_DIR})
string(REPLACE "\"" "" NODE_ADDON_API_DIR ${NODE_ADDON_API_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${NODE_ADDON_API_DIR})
add_definitions(-DNAPI_VERSION=5)