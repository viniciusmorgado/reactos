
include_directories(
    ${REACTOS_SOURCE_DIR}/sdk/include/reactos/libs/mbedtls)


add_definitions(-D_WINE)
spec2def(schannel.dll schannel.spec)

list(APPEND SOURCE
    lsamode.c
    schannel_mbedtls.c
    schannel_main.c
    schannel_wine.c
    secur32_wine.c
    usermode.c
    stubs.c
    precomp.h
    ${CMAKE_CURRENT_BINARY_DIR}/schannel_stubs.c)

add_library(schannel MODULE
    ${SOURCE}
    version.rc
    ${CMAKE_CURRENT_BINARY_DIR}/schannel.def)

set_module_type(schannel win32dll)
target_link_libraries(schannel wine)
add_importlibs(schannel mbedtls crypt32 secur32 advapi32 msvcrt kernel32 ntdll)
add_pch(schannel precomp.h SOURCE)
add_cd_file(TARGET schannel DESTINATION reactos/system32 FOR all)
set_wine_module(schannel)
