cmake_minimum_required(VERSION 3.14)
project(HalconMatching LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_INCLUDE_CURRENT_DIR ON) # include build directory where the qt generated header exists

set(Qt5_DIR C:/Qt13-visualstudio/5.13.0/msvc2017_64/lib/cmake/Qt5)
find_package(Qt5 REQUIRED COMPONENTS Widgets Core Gui)

# halcon
set(halcon_include_dirs "C:/Program Files/MVTec/HALCON-17.12-Progress/include")
set(halcon_libs "C:/Program Files/MVTec/HALCON-17.12-Progress/lib/x64-win64/halcon.lib;C:/Program Files/MVTec/HALCON-17.12-Progress/lib/x64-win64/halconcpp.lib")

# qt files
set(project_ui_files matching.ui qhalconwindow.ui)
set(project_src_files main.cpp matching.cpp qhalconwindow.cpp)
set(project_header_files matching.h qhalconwindow.h)

# moc related
set(CMAKE_AUTOMOC ON)
SET(CMAKE_AUTOUIC ON)

qt5_wrap_ui(project_headers_wrapped ${project_ui_files}) # ui files are converted to header files at build time like XML in WPF
qt5_wrap_cpp(project_src_moc ${project_header_files}) # because header files are where Q_Object is defined

add_executable(${PROJECT_NAME}  # ui files are not necessary here
        ${project_src_files}
#        ${project_header_files}
#        ${project_headers_wrapped}
        ${project_src_moc}
        )

target_link_libraries(${PROJECT_NAME} # PROJECT_NAME is case-sensitive
        PUBLIC
        Qt5::Core
        Qt5::Widgets
        Qt5::Gui
        ${halcon_libs}
        )

target_include_directories(${PROJECT_NAME}
        PUBLIC
        ${halcon_include_dirs}
        )

target_compile_definitions(${PROJECT_NAME}
        PUBLIC
        WIN32
        )