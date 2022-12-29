cmake_minimum_required(VERSION 3.24)

# Project settings ================================================================
project("libbcrypt"
        LANGUAGES C
        HOMEPAGE_URL "https://github.com/pk85-sp3ct0r/libbcrypt-cmake.git"
        )

set(CMAKE_C_STANDARD 23)
set(CMAKE_C_EXTENSIONS ON)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Fetch Mongooses Repository ======================================================
include(FetchContent)
FetchContent_Declare(
        crypt_blowfish
        GIT_REPOSITORY https://github.com/openwall/crypt_blowfish.git
        GIT_TAG Owl-3_1-release
)

FetchContent_MakeAvailable(crypt_blowfish)

add_library(crypt_blowfish INTERFACE)
target_sources(crypt_blowfish INTERFACE
        ${crypt_blowfish_SOURCE_DIR}/crypt_blowfish.c
        ${crypt_blowfish_SOURCE_DIR}/crypt_gensalt.c
        ${crypt_blowfish_SOURCE_DIR}/wrapper.c
        )
target_include_directories(crypt_blowfish INTERFACE ${crypt_blowfish_SOURCE_DIR})

add_library(${PROJECT_NAME} bcrypt.c bcrypt.h)
target_link_libraries(${PROJECT_NAME} crypt_blowfish)