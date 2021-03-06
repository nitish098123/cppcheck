if (BUILD_GUI AND BUILD_TESTS)
    set(CMAKE_AUTOMOC ON)
    if (CMAKE_BUILD_TYPE MATCHES "Release")
        add_definitions(-DQT_NO_DEBUG)
        add_definitions(-DQT_NO_DEBUG_OUTPUT)
        add_definitions(-DQT_NO_WARNING_OUTPUT)
    else()
        add_definitions(-DQT_DEBUG)
    endif()

    file(GLOB hdrs "*.h")
    file(GLOB srcs "*.cpp")
    file(GLOB uis  "*.ui")
    qt5_wrap_ui(uis_hdrs ${uis})

    add_executable(
        triage
        ${hdrs}
        ${srcs}
        ${uis_hdrs}
        ${PROJECT_SOURCE_DIR}/gui/codeeditorstyle.cpp
        ${PROJECT_SOURCE_DIR}/gui/codeeditor.cpp)
    target_include_directories(triage PUBLIC ${PROJECT_SOURCE_DIR}/gui/)
    target_link_libraries(triage Qt5::Core Qt5::Gui Qt5::Widgets)
    if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        if (NOT CMAKE_CXX_COMPILER_VERSION VERSION_LESS "8.0.0")
            # Q_UNUSED() in generated code
            target_compile_options(triage PRIVATE -Wno-extra-semi-stmt)
        endif()
    endif()

    set(CMAKE_AUTOMOC OFF)
endif()
