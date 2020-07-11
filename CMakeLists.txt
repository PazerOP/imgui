cmake_minimum_required(VERSION 3.13)

project(imgui)

if (POLICY CMP0077)
	# Allow CMake 3.13+ to override options when using FetchContent / add_subdirectory.
	cmake_policy(SET CMP0077 NEW)
endif ()

option(imgui_USE_OPENGL2 "Includes the OpenGL2 implementation." off)
option(imgui_USE_OPENGL3 "Includes the OpenGL3 implementation." off)
option(imgui_USE_GLBINDING "Uses glbinding for the OpenGL implementation." off)
option(imgui_USE_SDL2 "Uses the SDL2 implementation." off)

add_library(imgui
	"imconfig.h"
	"imgui_demo.cpp"
	"imgui_draw.cpp"
	"imgui_internal.h"
	"imgui_widgets.cpp"
	"imgui.cpp"
	"imgui.h"
	"imstb_rectpack.h"
	"imstb_textedit.h"
	"imstb_truetype.h"
	"misc/cpp/imgui_stdlib.cpp"
	"misc/cpp/imgui_stdlib.h"
)

if(imgui_USE_OPENGL2)
	target_sources(imgui PRIVATE "examples/imgui_impl_opengl2.cpp")
	target_sources(imgui PRIVATE "examples/imgui_impl_opengl2.h")
endif()

if(imgui_USE_OPENGL3)
	target_sources(imgui PRIVATE "examples/imgui_impl_opengl3.cpp")
	target_sources(imgui PRIVATE "examples/imgui_impl_opengl3.h")
endif()

if(imgui_USE_GLBINDING)
	find_package(glbinding CONFIG REQUIRED)
	target_link_libraries(imgui PRIVATE glbinding::glbinding)
endif()

if(imgui_USE_SDL2)
	find_package(SDL2 CONFIG REQUIRED)
	target_link_libraries(imgui PRIVATE SDL2::SDL2)

	target_sources(imgui PRIVATE "examples/imgui_impl_sdl.cpp")
	target_sources(imgui PRIVATE "examples/imgui_impl_sdl.h")
endif()

target_include_directories(imgui PUBLIC
	"."
	"misc/cpp"
)
