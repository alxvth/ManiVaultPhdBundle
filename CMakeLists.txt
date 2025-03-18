cmake_minimum_required(VERSION 3.30...3.31)

#       #
# Vcpkg #
#       #

if(DEFINED ENV{VCPKG_ROOT} AND NOT CMAKE_TOOLCHAIN_FILE)
    file(TO_CMAKE_PATH "$ENV{VCPKG_ROOT}" VCPKG_ROOT)
    message(STATUS "VCPKG_ROOT: ${VCPKG_ROOT}")
    set(CMAKE_TOOLCHAIN_FILE "${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")

    if(WIN32)
        set(VCPKG_HOST_TRIPLET "x64-windows-static-md")
        set(VCPKG_TARGET_TRIPLET "x64-windows-static-md")
        set(VCPKG_LIBRARY_LINKAGE "static")
    endif()

endif()

list(APPEND VCPKG_MANIFEST_FEATURES "runtimegpu")
message(STATUS "vcpkg: Adding to config: runtimegpu")

set(VCPKG_OVERLAY_PORTS "${CMAKE_CURRENT_LIST_DIR}/SpatialHierarchyPlugin/third_party/SpatialHierarchyLibrary/vcpkg-overlays/faiss")
message(STATUS "vcpkg: Using custom port: faiss")

project(ManiVaultPhdBundle
        VERSION 1.0.0
        LANGUAGES C CXX)

#         #
# General #
#         #

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    set(OpenMP_RUNTIME_MSVC "llvm" FORCE)
endif()

set(MV_INSTALL_DIR "${CMAKE_CURRENT_LIST_DIR}/install" CACHE PATH "Path where the MV core and plugins are installed")
set(MV_UNITY_BUILD ON CACHE BOOL "")
set(MV_USE_AVX ON CACHE BOOL "")

set(MV_SNE_USE_ARTIFACTORY_LIBS OFF)

#                        #
# ManiaVault and Plugins #
#                        #

# application
add_subdirectory(core)

# own (project) plugins
add_subdirectory(SpatialHierarchyPlugin)    # brings in HDILib
add_subdirectory(ImageEmbeddingCoupling)    # brings in HDILibSlim
#add_subdirectory(SpidrPlugin)              # TODO: update to use vcpkg for dependencies and interplay with above projects

# other plugins
add_subdirectory(ImageLoaderPlugin)
add_subdirectory(ImageViewerPlugin)
add_subdirectory(t-SNE-Analysis)
add_subdirectory(Scatterplot)
add_subdirectory(SpectralViewPlugin)

#                 #
# Dev environment #
#                 #

# visual studio settings
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT MV_Application)
endif()
