cmake_minimum_required(VERSION 3.5)
project(implot_vendor)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# TODO shouldn't need to repeat it, imgui_vendor should export setting
set(OpenGL_GL_PREFERENCE GLVND)

find_package(ament_cmake REQUIRED)
find_package(imgui_vendor REQUIRED)

add_library(implot SHARED
    implot/implot.cpp
    implot/implot_demo.cpp
    implot/implot_items.cpp)
target_include_directories(implot PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/implot>
  $<INSTALL_INTERFACE:include>)

ament_target_dependencies(implot
  "imgui_vendor")
ament_export_targets(export_implot HAS_LIBRARY_TARGET)

install(FILES
  implot/implot.h
  implot/implot_internal.h
  DESTINATION include)

install(TARGETS implot EXPORT export_implot
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

ament_export_include_directories(include)
ament_export_libraries(implot)
ament_export_dependencies(imgui_vendor)
ament_package()
