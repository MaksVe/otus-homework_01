cmake_minimum_required(VERSION 3.2)

project(homework_01 VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(version.h.in version.h)

add_executable(homework_01_cli main.cpp)

add_library(homework_01 lib.cpp)

add_executable(test_main_cli test_main.cpp)

set_target_properties(homework_01_cli test_main_cli PROPERTIES
	CXX_STANDART 14
	CXX_STANDART_REQUIRED ON
	COMPILE_OPTIONS -Wpedantic -Wall -Wextra)

set_target_properties(test_main_cli PROPERTIES
	COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
	INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR})

target_link_libraries(homework_01_cli homework_01)

target_link_libraries(test_main_cli
	${Boost_LIBRARIES}
	homework_01)

install(TARGETS homework_01_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT contact@contact.com)

include (CPack)

enable_testing()

add_test(test_version_valid test_main_cli)