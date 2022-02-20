cmake_minimum_required (VERSION 3.8)
project ("ImguiSkeleton")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
	# Without this, MSVC won't report the correct standards version via the
	# __cplusplus macro.
	#See: https://devblogs.microsoft.com/cppblog/msvc-now-correctly-reports-cplusplus/
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zc:__cplusplus /std:c++17")
endif()

#Set output folder to ./bin
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY $<1:${CMAKE_SOURCE_DIR}/bin>)

include_directories(
	include/
)

#Declare our target executable - put all source files here
add_executable (${PROJECT_NAME}
	src/ImguiSkeleton.cpp
	"src/AppWindow.cpp"
	"src/AppWindow.h")

#Add GLAD to recipe
add_library(GLAD STATIC
	thirdparty/glad.c
)
target_link_libraries(${PROJECT_NAME} GLAD)

#Add GLFW to recipe
add_subdirectory(thirdparty/glfw)
include_directories(thirdparty/glfw/include/) #Add Glfw headers
target_link_libraries(${PROJECT_NAME} glfw)

#Add ImGui to recipe
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLAD=1) #ImGui buildflag to indicate we're using GLAD
include_directories(thirdparty/imgui/) #Add ImGui headers
add_library(ImGui STATIC
	thirdparty/imgui/imgui.cpp
	thirdparty/imgui/imgui_demo.cpp
	thirdparty/imgui/imgui_draw.cpp
	thirdparty/imgui/imgui_tables.cpp
	thirdparty/imgui/imgui_widgets.cpp
	thirdparty/imgui/backends/imgui_impl_glfw.cpp
	thirdparty/imgui/backends/imgui_impl_opengl3.cpp
)
target_link_libraries(${PROJECT_NAME} ImGui)