cmake_minimum_required(VERSION 3.16)

project(breeze-gtk)
set(PROJECT_VERSION "6.7.5")

find_package(ECM 6.2.0 REQUIRED NO_MODULE)
include(FeatureSummary)

set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH}  "${CMAKE_SOURCE_DIR}/cmake")
find_package(GTKEngine)
find_package(Sass REQUIRED)
find_package(PythonCairo)
set_package_properties(PythonCairo PROPERTIES TYPE REQUIRED PURPOSE "Required to render assets")
find_package(SonicSilver 5.27.80 REQUIRED)
set_package_properties(SonicSilver PROPERTIES TYPE REQUIRED PURPOSE "Required to build the Silver GTK+ styles")


include(KDEInstallDirs)

add_subdirectory(src)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)

