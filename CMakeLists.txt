# Copyright (c) Alpaca Core
# SPDX-License-Identifier: MIT
#
cmake_minimum_required(VERSION 3.24 FATAL_ERROR)

project(tpl-ac-local-app
    VERSION 1.0.0
    DESCRIPTION "Example application which uses AC Local"
    LANGUAGES CXX
)

include(./get-ac-build.cmake)

include(init_ac_prj)
include(ac_build_prj_util)

#######################################
# packages

add_ac_local(0.1.18)

#######################################
# subdirs / targets
add_executable(${PROJECT_NAME}
    main.cpp
)
target_link_libraries(${PROJECT_NAME} PRIVATE
    ac::local
    ac::jalog
)

add_custom_target(run-${PROJECT_NAME} COMMAND ${PROJECT_NAME})

#######################################
# plugins

if(NOT AC_BUILD_DEPLOY)
    make_ac_local_plugin_available(
        NAME ilib-foo
        VERSION 0.1.2
        GITHUB alpaca-core/ilib-foo
    )
    target_link_libraries(${PROJECT_NAME} PRIVATE
        aclp::out-dir
    )
endif()
