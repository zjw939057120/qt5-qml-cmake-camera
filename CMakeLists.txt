cmake_minimum_required(VERSION 3.16)
project(Camera)

set(CMAKE_CXX_STANDARD 14)
#新增开始
if (WIN32)
    set(CMAKE_PREFIX_PATH "D:/Qt5/5.15.2/mingw81_64/lib/cmake")
else()
    set(CMAKE_PREFIX_PATH "/root/qt/qt-everywhere-src-5.15.9/_install/lib/cmake")
    set (CMAKE_C_COMPILER "/root/qt/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc")
    set (CMAKE_CXX_COMPILER "/root/qt/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-g++")
endif ()
find_package(Qt5 COMPONENTS Core Quick LinguistTools REQUIRED)
#INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})
set(SRC_FILES main.cpp)
qt5_add_resources(QRC_FILES  application.qrc)
add_executable(${PROJECT_NAME} WIN32 ${SRC_FILES} ${QRC_FILES})
#新增结束

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(QT_VERSION 5)
set(REQUIRED_LIBS Core Gui Widgets Quick Qml Multimedia)
set(REQUIRED_LIBS_QUALIFIED Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Quick Qt5::Qml Qt5::Multimedia)

#add_executable(${PROJECT_NAME} main.cpp)

if (NOT CMAKE_PREFIX_PATH)
    message(WARNING "CMAKE_PREFIX_PATH is not defined, you may need to set it "
            "(-DCMAKE_PREFIX_PATH=\"path/to/Qt/lib/cmake\" or -DCMAKE_PREFIX_PATH=/usr/include/{host}/qt{version}/ on Ubuntu)")
endif ()

find_package(Qt${QT_VERSION} COMPONENTS ${REQUIRED_LIBS} REQUIRED)
target_link_libraries(${PROJECT_NAME} ${REQUIRED_LIBS_QUALIFIED})
if (WIN32)
    set(DEBUG_SUFFIX)
    set(QT_INSTALL_PATH "${CMAKE_PREFIX_PATH}")
    if (NOT EXISTS "${QT_INSTALL_PATH}/bin")
        set(QT_INSTALL_PATH "${QT_INSTALL_PATH}/..")
        if (NOT EXISTS "${QT_INSTALL_PATH}/bin")
            set(QT_INSTALL_PATH "${QT_INSTALL_PATH}/..")
        endif ()
    endif ()
    if (EXISTS "${QT_INSTALL_PATH}/plugins/platforms/qwindows${DEBUG_SUFFIX}.dll")
        add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E make_directory
                "$<TARGET_FILE_DIR:${PROJECT_NAME}>/plugins/platforms/")
        add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy
                "${QT_INSTALL_PATH}/plugins/platforms/qwindows${DEBUG_SUFFIX}.dll"
                "$<TARGET_FILE_DIR:${PROJECT_NAME}>/plugins/platforms/")
    endif ()
endif ()
