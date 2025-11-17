cmake_minimum_required(VERSION 3.21)

include(cmake/Options.cmake)

if (USE_PROJECT_VCPKG)
    # vcpkg checked out as a git submodule at dependencies/vcpkg
    set(VCPKG_DIR "${CMAKE_CURRENT_LIST_DIR}/dependencies/vcpkg")
    set(VCPKG_TOOLCHAIN_FILE "${VCPKG_DIR}/scripts/buildsystems/vcpkg.cmake")

    if (NOT EXISTS "${VCPKG_TOOLCHAIN_FILE}")
        message(FATAL_ERROR
            "vcpkg toolchain file not found at: ${VCPKG_TOOLCHAIN_FILE}\n"
            "Did you initialize the submodule and bootstrap vcpkg?\n"
            "  git submodule update --init --recursive\n"
            "  ./scripts/setup_vcpkg.sh"
        )
    endif ()

    # Must be set before project() so CMake uses vcpkg for find_package()
    set(CMAKE_TOOLCHAIN_FILE "${VCPKG_TOOLCHAIN_FILE}"
        CACHE STRING "Vcpkg toolchain file" FORCE)
endif ()

project(Tuff LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
