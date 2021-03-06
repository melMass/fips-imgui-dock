#
# project: fips-imgui
#

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-imgui)
endif()
find_package(Freetype REQUIRED)
fips_begin_lib(imgui)
    fips_vs_disable_warnings(4312)   # 'type cast': conversion from 'int' to 'void *' of greater size
    fips_include_directories(${FREETYPE_INCLUDE_DIRS})
    fips_dir(imgui)
    fips_files(
        imconfig.h
        imgui.cpp
        imgui.h
        imgui_demo.cpp
        imgui_draw.cpp
        imgui_internal.h
        imgui_widgets.cpp
        imstb_rectpack.h
        imstb_textedit.h
        imstb_truetype.h
        misc/freetype/imgui_freetype.cpp
        misc/freetype/imgui_freetype.h
    )
    fips_frameworks_osx(Freetype)
    
fips_end_lib()

if (FIPS_CLANG)
    set_target_properties(imgui PROPERTIES COMPILE_FLAGS "-Wno-type-limits -Wno-missing-field-initializers")
elseif(FIPS_GCC)
    set_target_properties(imgui PROPERTIES COMPILE_FLAGS "-Wno-unused-but-set-variable -Wno-type-limits -Wno-missing-field-initializers")
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()

