cmake_minimum_required(VERSION 3.16)
project(lightly)
set(PROJECT_VERSION "6.80")
set(PROJECT_VERSION_MAJOR 6)


set(KF6_MIN_VERSION "5.240.0")
set(KDE_COMPILERSETTINGS_LEVEL "5.82")
set(QT_MIN_VERSION "6.6.0")

include(GenerateExportHeader)
include(WriteBasicConfigVersionFile)
include(FeatureSummary)
include(GNUInstallDirs)

find_package(ECM ${KF6_MIN_VERSION} REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH}  ${CMAKE_SOURCE_DIR}/cmake)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


include(CMakePackageConfigHelpers)
include(ECMInstallIcons)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(KDEClangFormat)
include(KDEGitCommitHooks)

set(QT_MAJOR_VERSION 6)
include(KDEInstallDirs6)
include(KDECMakeSettings)

find_package(Qt6 ${QT_MIN_VERSION} REQUIRED CONFIG COMPONENTS Widgets DBus)
find_package(KF6 ${KF6_MIN_VERSION} REQUIRED COMPONENTS
        CoreAddons
        ColorScheme
        Config
        GuiAddons
        I18n
        IconThemes
        WindowSystem
        )
        find_package(KF6KCMUtils ${KF6_MIN_VERSION})
        set_package_properties(KF6KCMUtils PROPERTIES
            TYPE REQUIRED
            DESCRIPTION "Helps create configuration modules"
            PURPOSE "KCMUtils used for the configuration modules or the decoration and Qt Style"
        )

find_package(Qt6 ${QT_MIN_VERSION} OPTIONAL_COMPONENTS Quick)
    if(${Qt6Quick_FOUND})
        find_package(KF6KirigamiPlatform ${KF6_MIN_VERSION} REQUIRED)
    endif()

find_package(KF6FrameworkIntegration ${KF6_MIN_VERSION} CONFIG )
    set_package_properties(KF${QT_MAJOR_VERSION}FrameworkIntegration PROPERTIES
        DESCRIPTION "KF6 Framework Integration"
        URL "https://projects.kde.org/projects/frameworks/frameworkintegration"
        TYPE OPTIONAL
        PURPOSE "Required to use KStyle convenience functionalities in style")

set(LIGHTLY_HAVE_KSTYLE ${KF6FrameworkIntegration_FOUND})
set(LIGHTLY_HAVE_QTQUICK ${Qt6Quick_FOUND})

add_subdirectory(kstyle kstyle6)
add_subdirectory(liblightlycommon liblightlycommon6)

ki18n_install(po)

# create a Config.cmake and a ConfigVersion.cmake file and install them
include(ECMSetupVersion)

set(CMAKECONFIG_INSTALL_DIR "${KDE_INSTALL_CMAKEPACKAGEDIR}/Lightly")

# add clang-format target for all our real source files
file(GLOB_RECURSE ALL_CLANG_FORMAT_SOURCE_FILES *.cpp *.h)
kde_clang_format(${ALL_CLANG_FORMAT_SOURCE_FILES})
kde_configure_git_pre_commit_hook(CHECKS CLANG_FORMAT)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
