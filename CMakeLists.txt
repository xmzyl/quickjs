# CMakeList.txt : CMake project for CMakeProject1, include source and define
# project specific logic here.
#
cmake_minimum_required(VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
    cmake_policy(SET CMP0141 NEW)
    set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif ()

project("qjs")


# Add source to this project's executable.
set(QJS_LIB_SRCS
        quickjs.c
        libregexp.c
        libunicode.c
        cutils.c
        quickjs-libc.c
        libbf.c
        repl.c
        qjscalc.c
)

set(QJS_SRCS
        qjs.c
        ${QJS_LIB_SRCS}
)

# Add source to this project's executable.
add_executable(qjs-debug ${QJS_SRCS})


file(READ "${CMAKE_SOURCE_DIR}/VERSION" VERSION_CONTENT)
string(STRIP "${VERSION_CONTENT}" VERSION_CONTENT)

target_compile_definitions(qjs-debug PRIVATE _GNU_SOURCE CONFIG_VERSION=\"${VERSION_CONTENT}\")
target_compile_definitions(qjs-debug PRIVATE CONFIG_BIGNUM)

if (WIN32)
    target_link_libraries(qjs-debug m pthread)
else ()
    target_link_libraries(qjs-debug m dl pthread)
endif ()

if (CMAKE_VERSION VERSION_GREATER 3.12)
    set_property(TARGET qjs-debug PROPERTY CXX_STANDARD 20)
endif ()

# TODO: Add tests and install targets if needed.