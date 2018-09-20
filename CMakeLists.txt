cmake_minimum_required(VERSION 3.12)
project(USAC_1_0)

set(CMAKE_CXX_STANDARD 14)

find_package(LAPACK REQUIRED)
include_directories(${LAPACK_INCLUDES})

set(LIBCONFIG_DIR ${CMAKE_SOURCE_DIR}/ext/libconfig-1.4.5)

set(LIBCONFIG_SRC ${LIBCONFIG_DIR}/lib/grammar.c
                  ${LIBCONFIG_DIR}/lib/libconfig.c
                  ${LIBCONFIG_DIR}/lib/libconfigcpp.cc
        ${LIBCONFIG_DIR}/lib/scanctx.c
        ${LIBCONFIG_DIR}/lib/scanner.c
        ${LIBCONFIG_DIR}/lib/strbuf.c)

include_directories(${LIBCONFIG_DIR})
add_library(config++ SHARED ${LIBCONFIG_SRC})

include_directories(${CMAKE_SOURCE_DIR}/src)
include_directories(${CMAKE_SOURCE_DIR}/src/config)
include_directories(${CMAKE_SOURCE_DIR}/src/estimators)
include_directories(${CMAKE_SOURCE_DIR}/src/utils)

add_library(usac SHARED
        src/config/ConfigFileReader.cpp
        src/config/ConfigParams.cpp
        src/config/ConfigParamsFundmatrix.cpp
        src/config/ConfigParamsHomog.cpp
        src/utils/FundmatrixFunctions.cpp
        src/utils/HomographyFunctions.cpp
        src/utils/MathFunctions.cpp)
target_link_libraries(usac config++  ${LAPACK_LIBRARIES})

add_executable(test msvc/RunSingleTest/RunSingleTest.cpp)
target_link_libraries(test usac)