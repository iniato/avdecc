project('l-acoustics/avdecc', ['c', 'cpp'],
  version: '3.2.5',
  default_options: ['cpp_std=c++17'],
  license: 'LGPL')


is_darwin = (target_machine.system() == 'darwin')

if is_darwin
    add_languages('objcpp')
endif

json_dep = dependency('nlohmann_json')
fmt_dep = dependency('fmt', required: false)
pcap_dep = dependency('pcap')
frameworks_dep = dependency('appleframeworks', modules: ['AudioVideoBridging', 'Foundation', 'CoreFoundation', 'CoreAudio'], required: is_darwin)
nih_dep = dependency('la_networkInterfaceHelper')

avdecc_include_dirs = include_directories('include', 'src')

avdecc_config_header = configure_file(
  input: 'src/config.h.in',
  output: 'config.h',
  configuration: {
    'CU_PROJECT_FRIENDLY_VERSION': meson.project_version(),
    'CU_PROJECT_COMPANYNAME': 'L-Acoustics',
    'CU_YEAR': 2024,
    'CU_PROJECT_LEGALCOPYRIGHT': 'Copyright 2016-2024, L-Acoustics and its contributors',
    'CU_PROJECT_READABLE_COPYRIGHT': 'Copyright 2016-2024, L-Acoustics and its contributors',
  })

avdecc_sources = {
  'common': files(
    'src/executor.cpp',
    'src/utils.cpp',
    'src/endStationImpl.hpp',
    'src/streamFormatInfo.cpp',
    'src/logHelper.hpp',
    # 'src/entity/localEntityImpl.ipp',
    'src/entity/localEntityImpl.cpp',
    'src/entity/entityModelTypes.cpp',
    'src/entity/controllerCapabilityDelegate.hpp',
    'src/entity/entityModelControlValues.cpp',
    'src/entity/controllerEntityImpl.cpp',
    'src/entity/aggregateEntityImpl.cpp',
    'src/entity/controllerEntityImpl.hpp',
    'src/entity/entityImpl.hpp',
    'src/entity/entityModelJsonSerializer.cpp',
    'src/entity/aggregateEntityImpl.hpp',
    'src/entity/controllerCapabilityDelegate.cpp',

    'src/stateMachine/advertiseStateMachine.hpp',
    'src/stateMachine/commandStateMachine.cpp',
    'src/stateMachine/discoveryStateMachine.cpp',
    'src/stateMachine/stateMachineManager.cpp',
    'src/stateMachine/advertiseStateMachine.cpp',
    'src/stateMachine/protocolInterfaceDelegate.hpp',
    'src/stateMachine/commandStateMachine.hpp',
    'src/stateMachine/discoveryStateMachine.hpp',
    'src/stateMachine/stateMachineManager.hpp',

    'src/protocol/protocolAemAecpdu.cpp',
    'src/protocol/protocolMvuAecpdu.cpp',
    'src/protocol/protocolAemControlValuesPayloads.hpp',
    'src/protocol/protocolAdpdu.cpp',
    'src/protocol/protocolAemPayloads.cpp',
    'src/protocol/protocolAaAecpdu.cpp',
    'src/protocol/protocolAecpdu.cpp',
    'src/protocol/protocolMvuPayloads.cpp',
    'src/protocol/protocolAvtpdu.cpp',
    'src/protocol/protocolAemPayloads.hpp',
    'src/protocol/protocolVuAecpdu.cpp',
    'src/protocol/protocolMvuPayloads.hpp',
    'src/protocol/protocolDefines.cpp',
    'src/protocol/protocolAcmpdu.cpp',
    'src/watchDog.cpp',

    # 'src/controller/avdeccControllerJsonTypes.hpp',
    # 'src/controller/avdeccEntityModelCache.hpp',
    # 'src/controller/avdeccControllerImpl.cpp',
    # 'src/controller/avdeccControllerImplHandlers.cpp',
    # 'src/controller/avdeccControlledEntityJsonSerializer.cpp',
    # 'src/controller/avdeccController.cpp',

    # # 'src/controller/pch.hpp',
    # 'src/controller/avdeccControlledEntityImpl.hpp',
    # 'src/controller/avdeccControllerImplOverrides.cpp',
    # # 'src/controller/config.h.in',
    # 'src/controller/avdeccControllerImplDelegateOverrides.cpp',
    # 'src/controller/avdeccControlledEntityJsonSerializer.hpp',
    # 'src/controller/avdeccControlledEntityImpl.cpp',
    # 'src/controller/avdeccControllerLogHelper.hpp',
    # 'src/controller/avdeccControllerImpl.hpp',
    'src/avdecc.cpp',
    'src/logger.cpp',
    # # 'src/bindings/c/executor_c.cpp',
    # # 'src/bindings/c/protocolAcmpdu_c.cpp',
    # # 'src/bindings/c/utils.cpp',
    # # 'src/bindings/c/CMakeLists.txt',
    # # 'src/bindings/c/localEntity_c.cpp',
    # # 'src/bindings/c/protocolInterface_c.hpp',
    # # 'src/bindings/c/avdecc_c.cpp',
    # # 'src/bindings/c/config.h.in',
    # # 'src/bindings/c/README.md',
    # # 'src/bindings/c/utils.hpp',
    # # 'src/bindings/c/protocolAdpdu_c.cpp',
    # # 'src/bindings/c/localEntity_c.hpp',
    # # 'src/bindings/c/protocolInterface_c.cpp',
    'src/protocolInterface/protocolInterface_pcap.cpp',
    'src/protocolInterface/protocolInterface_virtual.cpp',
    'src/protocolInterface/protocolInterface_virtual.hpp',
    # 'src/protocolInterface/pcapStaticLinking.cpp',
    'src/protocolInterface/protocolInterface_macNative.hpp',
    'src/protocolInterface/protocolInterface_pcap.hpp',
    'src/protocolInterface/pcapDynamicLinking.cpp',
    'src/protocolInterface/ethernetPacketDispatch.hpp',
    'src/protocolInterface/protocolInterface.cpp',
    'src/protocolInterface/pcapInterface.hpp',
    'src/endStationImpl.cpp',
    ),
  'darwin': files(
    'src/protocolInterface/protocolInterface_macNative.mm',
    ),
}

avdecc_cpp_args = [
  '-DLA_AVDECC_CONTROLLER_LIB_VERSION="@0@"'.format(meson.project_version()),
  fmt_dep.found() ? '-DHAVE_FMT=1' : [],

  is_darwin ? '-DHAVE_PROTOCOL_INTERFACE_MAC' : [],
  pcap_dep.found() ? '-DHAVE_PROTOCOL_INTERFACE_PCAP' : [],
  # '-DHAVE_PROTOCOL_INTERFACE_PROXY',
  '-DHAVE_PROTOCOL_INTERFACE_VIRTUAL',
  '-DENABLE_AVDECC_FEATURE_REDUNDANCY',
  '-DENABLE_AVDECC_FEATURE_JSON',
]

libavdecc = library('la-avdecc-cxx',
  sources: [
    avdecc_sources['common'],
    avdecc_sources.get(host_machine.system(), []),
  ],
  include_directories: avdecc_include_dirs,
  cpp_pch: 'src/pch.hpp',
  cpp_args: avdecc_cpp_args,
  dependencies: [nih_dep, json_dep, fmt_dep, pcap_dep, frameworks_dep],
  install: true,
  version: meson.project_version(),
  soversion: '3',
)

avdecc_dep = declare_dependency(
  include_directories: avdecc_include_dirs,
  link_with: libavdecc,
  dependencies: [nih_dep, json_dep])

meson.override_dependency('la_avdecc_cxx', avdecc_dep)
