# ============================================================================
# iogrpc — High-performance gRPC runtime for C23 on io_uring
# ============================================================================

cmake_minimum_required(VERSION 3.25)

project(iogrpc
    VERSION 0.1.0
    DESCRIPTION "High-performance gRPC runtime for C23 using io_uring"
    HOMEPAGE_URL "https://github.com/ioplane/iogrpc"
    LANGUAGES C
)

# ============================================================================
# C23 Standard
# ============================================================================

set(CMAKE_C_STANDARD 23)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# ============================================================================
# Build Options
# ============================================================================

option(IOGRPC_BUILD_TESTS "Build unit tests" ON)
option(IOGRPC_BUILD_EXAMPLES "Build examples" OFF)
option(IOGRPC_INSTALL "Generate install targets" ON)

# ============================================================================
# Compile Options
# ============================================================================

add_compile_options(
    -Wall -Wextra -Wpedantic -Werror
    -Wformat=2
    -Wconversion -Wsign-conversion
    -Wshadow -Wundef
    -fstack-protector-strong
    -fPIC
)

add_compile_definitions(_GNU_SOURCE)

# ============================================================================
# Dependencies
# ============================================================================

find_package(PkgConfig REQUIRED)

# core gRPC semantics and protobuf-c
pkg_check_modules(PROTOBUF_C REQUIRED libprotobuf-c)

# Transport dependencies (shared with iohttp)
pkg_check_modules(NGHTTP2 REQUIRED libnghttp2)
pkg_check_modules(URING REQUIRED liburing)
pkg_check_modules(WOLFSSL REQUIRED wolfssl)

# iohttp dependency (should be installed or in CMAKE_PREFIX_PATH)
# pkg_check_modules(IOHTTP REQUIRED iohttp)

# ============================================================================
# Library Sources
# ============================================================================

set(IOGRPC_SOURCES
    src/grpc_frame.c
    src/grpc_metadata.c
    src/grpc_status.c
    src/grpc_server.c
    src/grpc_stream.c
    src/grpc_h2_adapter.c
    src/grpc_codec_protobuf_c.c
)

# ============================================================================
# Targets
# ============================================================================

add_library(iogrpc STATIC ${IOGRPC_SOURCES})
target_include_directories(iogrpc
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_SOURCE_DIR}/src
)
target_link_libraries(iogrpc PRIVATE
    ${PROTOBUF_C_LIBRARIES}
    ${NGHTTP2_LIBRARIES}
    ${URING_LIBRARIES}
    ${WOLFSSL_LIBRARIES}
)
target_include_directories(iogrpc PRIVATE
    ${PROTOBUF_C_INCLUDE_DIRS}
    ${NGHTTP2_INCLUDE_DIRS}
    ${URING_INCLUDE_DIRS}
    ${WOLFSSL_INCLUDE_DIRS}
)

add_library(iogrpc::iogrpc ALIAS iogrpc)

# ============================================================================
# Testing (Unity)
# ============================================================================

if(IOGRPC_BUILD_TESTS)
    enable_testing()
    find_path(UNITY_INCLUDE_DIR unity/unity.h)
    find_path(UNITY_SRC_DIR unity.c PATH_SUFFIXES unity)
    if(UNITY_INCLUDE_DIR AND UNITY_SRC_DIR)
        add_library(unity STATIC ${UNITY_SRC_DIR}/unity.c)
        target_include_directories(unity PUBLIC ${UNITY_INCLUDE_DIR})
    endif()

    macro(iogrpc_add_test TEST_NAME TEST_SRC)
        if(TARGET unity)
            add_executable(${TEST_NAME} ${TEST_SRC})
            target_link_libraries(${TEST_NAME} PRIVATE unity iogrpc)
            add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME})
        endif()
    endmacro()

    # iogrpc_add_test(test_frame tests/test_frame.c)
endif()

# ============================================================================
# Quality & Install (omitted for brevity, follow iohttpparser template)
# ============================================================================
