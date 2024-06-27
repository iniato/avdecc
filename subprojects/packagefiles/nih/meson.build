project('l-acoustics/networkInterfaceHelper', ['c', 'cpp'],
  version: '1.1.0',
  default_options: ['cpp_std=c++17'],
  license: 'LGPL')

is_darwin = (target_machine.system() == 'darwin')

if is_darwin
    add_languages('objcpp')
endif

nih_include_dirs = include_directories('include')

nih_config_header = configure_file(
  input: 'src/config.hpp.in',
  output: 'config.hpp',
  configuration: {
    'PROJECT_NAME': meson.project_name(),
    'CU_PROJECT_FULL_NAME': 'Network InterfaceHelper Library',
    'CU_COMPANY_NAME': 'L-Acoustics',
    'CU_COMPANY_URL': 'http://www.l-acoustics.com',
    'CU_PROJECT_CONTACT': 'software@l-acoustics.com',
    'CU_PROJECT_FRIENDLY_VERSION': meson.project_version(),
    'CU_PROJECT_MARKETING_VERSION': meson.project_version(),
    'CU_PROJECT_CMAKEVERSION_STRING': meson.project_version(),
    'CU_PROJECT_LEGALCOPYRIGHT': 'Copyright 2016-2024, L-Acoustics and its contributors',
    'CU_PROJECT_READABLE_COPYRIGHT': 'Copyright 2016-2024, L-Acoustics and its contributors',
    'CU_TARGET_ARCH': host_machine.cpu_family(),
    'CU_BUILD_NUMBER': 1337,
  })

nih_sources = {
  'common': files(
    'src/ipAddress.cpp',
    'src/ipAddressInfo.cpp',
    'src/libraryInfo.cpp',
    'src/networkInterfaceHelper_common.cpp',
    'src/networkInterfaceHelper_common.hpp',
    ),
  'darwin': files(
    #'src/networkInterfaceHelper.i',
    #'src/networkInterfaceHelper_ios.mm',
    'src/networkInterfaceHelper_mac.mm',
    ),
  'linux': files(
    'src/networkInterfaceHelper_unix.cpp',
    ),
  'windows': files(
    'src/networkInterfaceHelper_win32.cpp',
    ),
}

libnih = library('la-networkInterfaceHelper',
  sources: [
    nih_config_header,
    nih_sources['common'],
    nih_sources.get(host_machine.system(), []),
  ],
  include_directories: nih_include_dirs,
  dependencies: [dependency('appleframeworks', modules: ['Foundation', 'CoreFoundation', 'IOKit', 'SystemConfiguration'], required: is_darwin)],
  install: true,
  version: meson.project_version(),
  soversion: '1',
)

nih_dep = declare_dependency(
  include_directories: nih_include_dirs,
  link_with: libnih)

meson.override_dependency('la_networkInterfaceHelper', nih_dep)
