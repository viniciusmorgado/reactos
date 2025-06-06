
add_definitions(
    -D__ROS_LONG64__
    -D_USE_MATH_DEFINES)

spec2def(gdiplus.dll gdiplus.spec ADD_IMPORTLIB)

list(APPEND SOURCE
    brush.c
    customlinecap.c
    font.c
    gdiplus.c
    graphics.c
    graphicspath.c
    image.c
    imageattributes.c
    matrix.c
    metafile.c
    pathiterator.c
    pen.c
    region.c
    stringformat.c)

list(APPEND PCH_SKIP_SOURCE
    guid.c
    ${CMAKE_CURRENT_BINARY_DIR}/gdiplus_stubs.c)

add_library(gdiplus MODULE
    ${SOURCE}
    ${PCH_SKIP_SOURCE}
    gdiplus.rc
    ${CMAKE_CURRENT_BINARY_DIR}/gdiplus.def)

if(MSVC)
    # Disable warning C4146: unary minus operator applied to unsigned type, result still unsigned
    # Disable warning C4305: '=': truncation from 'double' to 'REAL'
    # Disable warning C5286: implicit conversion from enum type 'x' to enum type 'y';
    target_compile_options(gdiplus PRIVATE /wd4146 /wd4305 /wd5286)
endif()

set_module_type(gdiplus win32dll)
target_link_libraries(gdiplus wine)
add_delay_importlibs(gdiplus windowscodecs)
add_importlibs(gdiplus msvcrt shlwapi oleaut32 ole32 user32 gdi32 kernel32 ntdll)
add_pch(gdiplus precomp.h "${PCH_SKIP_SOURCE}")
add_cd_file(TARGET gdiplus DESTINATION reactos/system32 FOR all)

add_cd_file(TARGET gdiplus DESTINATION reactos/winsxs/${WINARCH}_microsoft.windows.gdiplus_6595b64144ccf1df_1.1.7601.23038_none_deadbeef FOR all)
add_cd_file(FILE ${CMAKE_CURRENT_SOURCE_DIR}/${WINARCH}_microsoft.windows.gdiplus_6595b64144ccf1df_1.1.7601.23038_none_deadbeef.manifest DESTINATION reactos/winsxs/manifests FOR all)

add_cd_file(TARGET gdiplus DESTINATION reactos/winsxs/${WINARCH}_microsoft.windows.gdiplus_6595b64144ccf1df_1.0.14393.0_none_deadbeef FOR all)
add_cd_file(FILE ${CMAKE_CURRENT_SOURCE_DIR}/${WINARCH}_microsoft.windows.gdiplus_6595b64144ccf1df_1.0.14393.0_none_deadbeef.manifest DESTINATION reactos/winsxs/manifests FOR all)
set_wine_module_FIXME(gdiplus) # CORE-5743: No ARRAY_SIZE macro
