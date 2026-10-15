# Set the C++ standard and whether you want extensions enabled.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(CMAKE_CXX_EXTENSIONS OFF)

# Add the library/executable target(s).
add_executable(${PNAME})

#-------------------------------------------------------------------------------

# Add source files to the target.
target_sources(
    ${PNAME} PRIVATE
    main.cpp
)

# Set the output directory for the target.
set_target_properties(
    ${PNAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${OUTPUT_DIRECTORY}
    LIBRARY_OUTPUT_DIRECTORY ${OUTPUT_DIRECTORY}
    ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_DIRECTORY}
)

# Add any necessary include directories.
# target_include_directories(
#     ${PNAME} PRIVATE
# )

# Add compiler flags by compiler toolchain.
target_compile_options(
    ${PNAME} PRIVATE
    $<$<CXX_COMPILER_ID:Clang,GNU>:
        -Wall
        -Wextra
        -Wpedantic
    >
    $<$<CXX_COMPILER_ID:GNU>:
        $<$<NOT:$<PLATFORM_ID:Windows>>:
            $<$<CONFIG:Debug>:-ggdb>
        >
    >
    $<$<CXX_COMPILER_ID:Clang>:
        $<$<NOT:$<PLATFORM_ID:Windows>>:
            $<$<CONFIG:Debug>:-glldb>
        >
    >
    $<$<CXX_COMPILER_ID:MSVC>:
        /W3
        $<$<CONFIG:Debug>:/Zi>
    >
)

# Add definitions to the compiler flags. These are macros which can be used from
# code to control features or compile code conditionally.
target_compile_definitions(
    ${PNAME} PRIVATE
    $<$<CONFIG:Debug>:DEBUG>
)
