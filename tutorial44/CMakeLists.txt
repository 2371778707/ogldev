cmake_minimum_required (VERSION 2.8)

add_definitions ("-std=c++11")

project (tutorial44)

set (SOURCES
	lighting_technique.cpp
	mesh.cpp
	tutorial44.cpp
)

set (RESOURCES shaders/lighting.fs shaders/lighting.vs) # appicon.icns

add_executable (${PROJECT_NAME} ${SOURCES} ${RESOURCES})

add_library (ogldev STATIC
	../common/camera.cpp
	../common/glut_backend.cpp
	../common/math_3d.cpp
	../common/ogldev_app.cpp
	../common/ogldev_backend.cpp
	../common/ogldev_basic_lighting.cpp
	../common/ogldev_basic_mesh.cpp
	../common/ogldev_glfw_backend.cpp
	../common/ogldev_texture.cpp
	../common/ogldev_util.cpp
	../common/pipeline.cpp
	../common/technique.cpp
)
target_link_libraries (${PROJECT_NAME} ogldev)

add_library (freetypeGL STATIC
	../common/FreetypeGL/font-manager.c
	../common/FreetypeGL/font_shader.cpp
	../common/FreetypeGL/freetypeGL.cpp
	../common/FreetypeGL/texture-atlas.c
	../common/FreetypeGL/texture-font.c
	../common/FreetypeGL/texture-glyph.c
	../common/FreetypeGL/vector.c
	../common/FreetypeGL/vertex-buffer.c
)
include_directories (../common ../common/FreetypeGL)
target_link_libraries (${PROJECT_NAME} freetypeGL)

target_link_libraries (${PROJECT_NAME} glut)

find_package (OpenGL REQUIRED)
target_link_libraries (${PROJECT_NAME} ${OPENGL_LIBRARIES})

include (FindPkgConfig)

pkg_check_modules (MAGICK REQUIRED Magick++)
include_directories (${MAGICK_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${MAGICK_LIBRARIES})

pkg_check_modules (FREETYPE REQUIRED freetype2)
include_directories (${FREETYPE_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${FREETYPE_LIBRARIES})

pkg_check_modules (GLFW REQUIRED glfw3)
include_directories (${GLFW_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${GLFW_LIBRARIES})

pkg_check_modules (ASSIMP REQUIRED assimp)
include_directories (${ASSIMP_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${ASSIMP_LIBRARIES})

pkg_check_modules (FONTCONFIG REQUIRED fontconfig)
include_directories (${FONTCONFIG_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${FONTCONFIG_LIBRARIES})

if (APPLE)
    set_source_files_properties (${RESOURCES} PROPERTIES MACOSX_PACKAGE_LOCATION Resources)
    set_target_properties (${PROJECT_NAME} PROPERTIES MACOSX_BUNDLE TRUE)
    # set (MACOSX_BUNDLE_ICON_FILE appicon)
endif (APPLE)

if (UNIX)
    pkg_search_module (GLEW REQUIRED glew)
    include_directories (${GLEW_INCLUDE_DIRS})
    target_link_libraries (${PROJECT_NAME} ${GLEW_LIBRARIES})
    add_definitions(-DUSE_GLEW)
endif ()
