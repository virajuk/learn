cmake_minimum_required(VERSION 3.21)
project(FIRST LANGUAGES CXX)

add_executable(${PROJECT_NAME} main.cpp)

add_subdirectory(Adder)
target_include_directories(${PROJECT_NAME} PUBLIC Adder)
target_link_libraries(${PROJECT_NAME} adder)

# message(STATUS "ECHO: ${PROJECT_SOURCE_DIR}")

find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
    option(GIT_SUBMODULE "Check submodules during build" ON)
    if(GIT_SUBMODULE)
        message(STATUS "Submodule update")
        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                        RESULT_VARIABLE GIT_SUBMOD_RESULT)
        if(NOT GIT_SUBMOD_RESULT EQUAL "0")
            message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please check ...")           
        endif()
    endif()
endif()
add_subdirectory(ext/glfw)
target_include_directories(${PROJECT_NAME} PUBLIC ext/glfw/include)
target_link_libraries(${PROJECT_NAME} glfw)

