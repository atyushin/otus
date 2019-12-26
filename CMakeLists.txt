cmake_minimum_required(VERSION 3.10)

project(helloWorld VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(
version.h.in
${CMAKE_CURRENT_SOURCE_DIR}/version.h
)

set (SOURCES 
main.cpp
)

set (HEADERS 
version.h
)

add_executable(helloWorld ${SOURCES} ${HEADERS})
add_executable(test_version test_version.cpp)

set_target_properties(
helloWorld PROPERTIES
CXX_STANDART 11)

set_target_properties(test_version PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries(test_version
    ${Boost_LIBRARIES}
)

install(TARGETS helloWorld RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT blinkers@mail.ru)

include(CPack)

enable_testing()

add_test(test_version test_version)
