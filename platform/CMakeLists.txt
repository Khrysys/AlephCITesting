include("${CMAKE_SOURCE_DIR}/cmake/target_macros.cmake")

aleph_add_library(Platform STATIC SOURCES 
    "include/aleph/platform/allocation.hpp"
    "include/aleph/platform/compiler.hpp"
    "include/aleph/platform/hash.hpp"
    "include/aleph/platform/intrinsics.hpp"
    "include/aleph/platform/threads.hpp"
    "include/aleph/platform.hpp"
    "src/allocation.cpp"
    "src/intrinsics.cpp"
    "src/os_dependent.hpp"
    "src/threads.cpp"
)

# Applies a compiler-specific architecture flag if the given option is enabled.
# GCC/Clang use -m<flag> style; MSVC uses /arch: style.
# Note: BMI2 has no dedicated MSVC flag — it is implied by /arch:AVX2.
macro(aleph_arch_option enabled gccflag winflag)
    if(${enabled})
        target_compile_options(aleph_platform PUBLIC
            $<$<CXX_COMPILER_ID:GNU,Clang>:${gccflag}>
            $<$<CXX_COMPILER_ID:MSVC>:${winflag}>
        )
    endif()
endmacro()

target_link_libraries(aleph_platform PUBLIC Boost::headers fmt::fmt libassert::assert)

if(libnuma_FOUND)
    target_link_libraries(aleph_platform PUBLIC libnuma::libnuma)
endif()

add_subdirectory(benchmarks)
add_subdirectory(fuzzing)
add_subdirectory(tests)

if(Aleph_INSTALL)
    include(GNUInstallDirs)
    install(DIRECTORY include/aleph DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
endif()