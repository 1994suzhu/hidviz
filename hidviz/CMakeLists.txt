cmake_minimum_required(VERSION 3.2)
project(hidviz VERSION 0.1.4 LANGUAGES C CXX)

# Find the QtWidgets library
find_package(Qt5 REQUIRED COMPONENTS Widgets)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

add_executable(hidviz WIN32
        src/main.cc
        src/DeviceSelector.cc
        src/Window.cc
        src/DeviceSelectionListItem.cc
        src/hid/ItemWidget.cc
        src/hid/CollectionWidget.cc
        src/hid/ControlWidget.cc
        src/FlowLayout.cc
        src/DeviceView.cc
        images/images.qrc
        src/hid/UsageWidget.cc src/hid/UsageWidget.hh
        src/WaitDialog.cc src/WaitDialog.hh
        src/Global.hh src/WindowsDriverDialog.cc src/WindowsDriverDialog.hh)

target_link_libraries(hidviz
        hidx
        Qt5::Widgets
        )
target_compile_features(hidviz PUBLIC
        cxx_relaxed_constexpr #this should force c++14
        )
target_compile_options(hidviz PRIVATE $<$<OR:$<CXX_COMPILER_ID:GNU>,$<CXX_COMPILER_ID:Clang>>: -Wall -Wextra>)

target_include_directories(hidviz PUBLIC ${CMAKE_CURRENT_BINARY_DIR})


if(NOT WIN32)
    include(GNUInstallDirs)
    install(TARGETS hidviz
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            )
    set_target_properties(hidviz PROPERTIES INSTALL_RPATH ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/hidviz)
endif()
