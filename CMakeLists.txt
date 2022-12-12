# cmake -S . -B build [--trace-expand]
# cmake --build build --config RelWithDebInfo
cmake_minimum_required(VERSION 3.5)

project(fact LANGUAGES CXX VERSION 0.0.1)

function(prepareTestLibrary)
  find_package(Catch2 QUIET)
  if(NOT Catch2_FOUND)
    message(STATUS "Fetching Catch2")
    include(FetchContent)

    FetchContent_Declare(
      CATCH2
      GIT_REPOSITORY https://github.com/catchorg/Catch2.git
      GIT_TAG v2.13.8)

    FetchContent_MakeAvailable(Catch2)

    message(STATUS "catch2_SOURCE_DIR: ${catch2_SOURCE_DIR}")

    # For include(Catch)
    list(APPEND CMAKE_MODULE_PATH "${Catch2_SOURCE_DIR}/contrib")
    message(STATUS "CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}")
  endif()

  include(CTest)
  include(Catch)
endfunction()

set(CMAKE_CXX_STANDARD 11)

preparetestlibrary()

# main executable (fact)
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE libfact)

# library (libfact)
add_subdirectory(libfact)

# enable_testing() is essential for ctest to find tests!
enable_testing()

# test (fact_test)
add_executable(fact_test fact_test.cpp)
target_link_libraries(fact_test PRIVATE libfact Catch2::Catch2)

catch_discover_tests(fact_test)

