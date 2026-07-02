cmake_minimum_required(VERSION 3.16)

project(xparser C)

# ----------------------------------------
# Compiler settings
# ----------------------------------------
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_compile_options(-Wall -pedantic)

# Debug flags (-g like your Makefile)
#set(CMAKE_BUILD_TYPE Debug)

# ----------------------------------------
# Sources
# ----------------------------------------
set(SOURCES
    xparser.c
    memory.c
    readmodel.c
    dependencygraph.c
    parsetemplate.c
)

# ----------------------------------------
# Executable
# ----------------------------------------
add_executable(xparser ${SOURCES})

# ----------------------------------------
# Custom targets (matching Makefile)
# ----------------------------------------

# `make test`
add_custom_target(test
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR}
    COMMAND ${CMAKE_MAKE_PROGRAM} --directory=tests
)

# `make testclean`
add_custom_target(testclean
    COMMAND ${CMAKE_COMMAND} -E rm -f
        tests/*.o
        tests/run_test.exe
        tests/run_test
        tests/stderr.out
        tests/stdout.out
)

# Docs
add_custom_target(docs
    DEPENDS user_docs developer_docs
)

add_custom_target(user_docs
    COMMAND ${CMAKE_MAKE_PROGRAM} --directory=docs/user_manual
)

add_custom_target(developer_docs
    COMMAND ${CMAKE_MAKE_PROGRAM} --directory=docs/developer_manual
)

add_custom_target(docsclean
    COMMAND ${CMAKE_MAKE_PROGRAM} --directory=docs/user_manual clean
    COMMAND ${CMAKE_MAKE_PROGRAM} --directory=docs/developer_manual clean
)

# Clean (extra beyond default CMake clean)
add_custom_target(vclean
    DEPENDS testclean docsclean
)

