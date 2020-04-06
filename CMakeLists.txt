cmake_minimum_required( VERSION 3.12.0 )

project(libbase64)

set(LIB base64)

include_directories(include)


set(LIB_SOURCES
    lib/config.h
    include/libbase64.h

    lib/lib.c
    lib/env.h
    lib/codecs.h
    lib/codec_choose.c

    lib/tables/table_enc_12bit.h
    lib/tables/tables.c
    lib/tables/tables.h
    lib/tables/table_generator.c
    lib/tables/table_dec_32bit.h

    lib/arch/avx/codec.c
    lib/arch/sse42/codec.c
    lib/arch/neon64/codec.c
    lib/arch/avx2/codec.c
    lib/arch/sse41/codec.c
    lib/arch/ssse3/codec.c
    lib/arch/neon32/codec.c
    lib/arch/generic/codec.c
   )


list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(flags)
include(cpp)


#Library
add_library(${LIB} STATIC ${LIB_SOURCES})
target_compile_options(${LIB} PRIVATE -Wno-implicit-fallthrough)


#Install
install(TARGETS ${LIB} ARCHIVE DESTINATION lib CONFIGURATIONS Release)
install(FILES include/libbase64.h DESTINATION include)
