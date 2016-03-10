
if(WIN32)
cmake_minimum_required (VERSION 3.1)
else()
cmake_minimum_required (VERSION 3.0)
endif()

project(multi-object-single-buffer-tutorial)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set (multi-object-single-buffer-tutorial_VERSION_MAJOR 1)
set (multi-object-single-buffer-tutorial_VERSION_MINOR 0)
set (multi-object-single-buffer-tutorial_VERSION_BUILD 0)

FILE(GLOB SOURCE_FILES
  src/main.cpp
)
source_group("Source Files" FILES ${SOURCE_FILES})

add_executable(multi-object-single-buffer-tutorial ${SOURCE_FILES})

if (MSVC)
    file(
        COPY ${CMAKE_SOURCE_DIR}/lib/glew-1.13.0-win32.zip
        DESTINATION ${CMAKE_BINARY_DIR}
    )
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_BINARY_DIR}/glew-1.13.0-win32.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )
    file(
        COPY ${CMAKE_SOURCE_DIR}/lib/glfw-3.1.2.bin.WIN32.zip
        DESTINATION ${CMAKE_BINARY_DIR}
    )
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_BINARY_DIR}/glfw-3.1.2.bin.WIN32.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )
    file(
        COPY ${CMAKE_SOURCE_DIR}/lib/glm-0.9.7.1.zip
        DESTINATION ${CMAKE_BINARY_DIR}
    )
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_BINARY_DIR}/glm-0.9.7.1.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    target_link_libraries(multi-object-single-buffer-tutorial
      ${CMAKE_BINARY_DIR}/glew-1.13.0/lib/Release/Win32/glew32.lib
      ${CMAKE_BINARY_DIR}/glfw-3.1.2.bin.WIN32/lib-vc2015/glfw3.lib
      opengl32.lib
    )

    include_directories(
        ${CMAKE_BINARY_DIR}/glew-1.13.0/include
        ${CMAKE_BINARY_DIR}/glfw-3.1.2.bin.WIN32/include
        ${CMAKE_BINARY_DIR}/glm
    )

    add_custom_command(TARGET multi-object-single-buffer-tutorial POST_BUILD        # Adds a post-build event to MyTest
        COMMAND ${CMAKE_COMMAND} -E copy_if_different  # which executes "cmake - E copy_if_different..."
            "${CMAKE_BINARY_DIR}/glew-1.13.0/bin/Release/Win32/glew32.dll"      # <--this is in-file
            $<TARGET_FILE_DIR:multi-object-single-buffer-tutorial>)
endif (MSVC)
# vim: ts=4 sw=4 et
