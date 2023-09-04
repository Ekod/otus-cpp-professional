cmake_minimum_required(VERSION 3.10)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(otus_cpp_professional VERSION ${PROJECT_VERSION})

add_subdirectory(tests)

configure_file(version.h.in version.h)

add_executable(otus_cpp_professional_cli main.cpp)
add_library(otus_cpp_professional lib.cpp)
add_executable(tests tests/test_version.cpp)
enable_testing()

set_target_properties(otus_cpp_professional_cli otus_cpp_professional tests PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)
target_include_directories(otus_cpp_professional
    PRIVATE "${CMAKE_BINARY_DIR}"
)

target_link_libraries(otus_cpp_professional_cli PRIVATE
        otus_cpp_professional
)


if (MSVC)
    target_compile_options(otus_cpp_professional_cli PRIVATE
        /W4
    )
    target_compile_options(otus_cpp_professional PRIVATE
        /W4
    )
    target_compile_options(tests PRIVATE
        /W4
    )
else ()
    target_compile_options(otus_cpp_professional_cli PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(otus_cpp_professional PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(tests PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()

install(TARGETS otus_cpp_professional_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)


