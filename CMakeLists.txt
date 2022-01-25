cmake_minimum_required(VERSION 3.20)
project(atomscistudio LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(QT_VERSION_MAJOR 6)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Widgets Core OpenGL Gui 3DCore 3DRender 3DInput 3DExtras Concurrent REQUIRED)
#find_package(OpenGL REQUIRED)
#find_package(GLUT REQUIRED)
#include_directories( ${OPENGL_INCLUDE_DIRS}  ${GLUT_INCLUDE_DIRS} )


include_directories(${QT_QTOPENGL_INCLUDE_DIR} ${OPENGL_INCLUDE_DIR} )

find_package(Armadillo REQUIRED)
include_directories(${ARMADILLO_INCLUDE_DIRS})
link_libraries(${ARMADILLO_LIBRARIES})

include_directories("./src")

find_package(atomsciflow REQUIRED)
include_directories(${ATOMSCIFLOW_INCLUDE_DIR})
link_libraries(${ATOMSCIFLOW_LIBRARIES})


file(GLOB PROJECT_SOURCES
    ./src/main.cpp

    ./src/main/*.h
    ./src/main/*.cpp

    ./src/modeling/*.h
    ./src/modeling/*.cpp
)


if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(atomscistudio
        ${PROJECT_SOURCES}
    )
else()
    if(ANDROID)
        add_library(atomscistudio SHARED
            ${PROJECT_SOURCES}
        )
    else()
        add_executable(atomscistudio
            ${PROJECT_SOURCES}
        )
    endif()
endif()


target_link_libraries(atomscistudio
    PRIVATE Qt${QT_VERSION_MAJOR}::Widgets
    PRIVATE Qt${QT_VERSION_MAJOR}::OpenGL
    PRIVATE Qt${QT_VERSION_MAJOR}::Gui
    PRIVATE Qt${QT_VERSION_MAJOR}::3DCore
    PRIVATE Qt${QT_VERSION_MAJOR}::3DRender
    PRIVATE Qt${QT_VERSION_MAJOR}::3DInput
    PRIVATE Qt${QT_VERSION_MAJOR}::3DExtras
    PRIVATE Qt${QT_VERSION_MAJOR}::Concurrent
    #${OPENGL_LIBRARIES}
    #${GLUT_LIBRARY}
)

# ------------------------------------------------
#                   set install
# ------------------------------------------------
install(
    TARGETS atomscistudio
    EXPORT atomscistudio
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)
