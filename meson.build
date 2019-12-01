project(
  'jack2',
  ['c', 'cpp'],
  default_options: [
    'prefix=/usr/local',
    'b_pie=true',
    'b_lto=true'
  ],
  meson_version: '>=0.50.0',
  license: ['GPL2+', 'LGPL2.1'],
  version: '1.9.14')

jack_api_version = '0.1.0'
os = build_machine.system()
conf_data = configuration_data()
conf_data.set('version', meson.project_version())
conf_data.set('addon_dir', join_paths(get_option('prefix'), get_option('libdir'), '/jack'))
config_header = configure_file(
  input: 'common/config.h.in',
  output: 'config.h',
  configuration: conf_data)

lib_dl = meson.get_compiler('c').find_library('dl')
lib_m = meson.get_compiler('c').find_library('m')
lib_rt = meson.get_compiler('c').find_library('rt')
lib_db = meson.get_compiler('c').find_library('db')
lib_c = meson.get_compiler('c').find_library('libc')
lib_stdcpp = meson.get_compiler('cpp').find_library('stdc++')

if os == 'sunos'
  dep_oss = dependency('oss')
endif

if os == 'darwin'
  dep_accelerate = dependency('Accelerate')
  dep_audiounit = dependency('AudioUnit')
  dep_audiotoolbox = dependency('AudioToolbox')
  dep_coreaudio = dependency('CoreAudio')
  dep_corefoundation = dependency('CoreFoundation')
  dep_coreservices = dependency('CoreServices')
endif

if os == 'windows'
  dep_regex = dependency('REGEX')
  dep_ws2_32 = dependency('WS2_32')
  dep_psapi = dependency('PSAPI')
  dep_portaudio = dependency('portaudio')
  dep_winmm = dependency('WINMM')
endif

dep_alsa = dependency('alsa', version: '>=1.0.18', required: get_option('alsa'))
dep_celt = dependency('celt', version: '>=0.11.0')
dep_dbus = dependency('dbus-1', version: '>=1.0.0', required: get_option('dbus'))
dep_ffado = dependency('libffado', version: '>=1.999.17', required: get_option('ffado'))
dep_opus = dependency('opus', version: '>=0.9.0')
dep_readline = dependency('readline')
dep_samplerate = dependency('samplerate')
dep_sndfile = dependency('sndfile')
dep_systemd = dependency('systemd', required: get_option('systemd'))
dep_threads = dependency('threads')

doxygen = find_program('doxygen', required : get_option('doxygen'))

# sources for libjack
src_common = [
  'common/JackActivationCount.cpp',
  'common/JackAPI.cpp',
  'common/JackClient.cpp',
  'common/JackConnectionManager.cpp',
  'common/ringbuffer.c',
  'common/JackError.cpp',
  'common/JackException.cpp',
  'common/JackFrameTimer.cpp',
  'common/JackGraphManager.cpp',
  'common/JackPort.cpp',
  'common/JackPortType.cpp',
  'common/JackAudioPort.cpp',
  'common/JackMidiPort.cpp',
  'common/JackMidiAPI.cpp',
  'common/JackEngineControl.cpp',
  'common/JackShmMem.cpp',
  'common/JackGenericClientChannel.cpp',
  'common/shm.c',
  'common/JackGlobals.cpp',
  'common/JackTransportEngine.cpp',
  'common/JackTools.cpp',
  'common/JackMessageBuffer.cpp',
  'common/JackEngineProfiling.cpp']
src_jack_client = [
  'common/JackLibClient.cpp',
  'common/JackLibAPI.cpp',
  'common/JackMetadata.cpp']
src_jack_client_posix = [
  'posix/JackSocketClientChannel.cpp',
  'posix/JackPosixServerLaunch.cpp']
src_jack_client_windows = [
  'windows/JackWinNamedPipe.cpp',
  'windows/JackWinNamedPipeClientChannel.cpp',
  'windows/JackWinServerLaunch.cpp',
  'windows/JackMMCSS.cpp']

# sources for libjackserver
src_jack_server = [
  'common/JackAudioDriver.cpp',
  'common/JackTimedDriver.cpp',
  'common/JackMidiDriver.cpp',
  'common/JackDriver.cpp',
  'common/JackEngine.cpp',
  'common/JackExternalClient.cpp',
  'common/JackFreewheelDriver.cpp',
  'common/JackInternalClient.cpp',
  'common/JackInternalSessionLoader.cpp',
  'common/JackServer.cpp',
  'common/JackThreadedDriver.cpp',
  'common/JackRestartThreadedDriver.cpp',
  'common/JackWaitThreadedDriver.cpp',
  'common/JackWaitCallbackDriver.cpp',
  'common/JackServerAPI.cpp',
  'common/JackDriverLoader.cpp',
  'common/JackServerGlobals.cpp',
  'common/JackControlAPI.cpp',
  'common/JackNetTool.cpp',
  'common/JackNetInterface.cpp',
  'common/JackArgParser.cpp',
  'common/JackRequestDecoder.cpp',
  'common/JackMidiAsyncQueue.cpp',
  'common/JackMidiAsyncWaitQueue.cpp',
  'common/JackMidiBufferReadQueue.cpp',
  'common/JackMidiBufferWriteQueue.cpp',
  'common/JackMidiRawInputWriteQueue.cpp',
  'common/JackMidiRawOutputWriteQueue.cpp',
  'common/JackMidiReadQueue.cpp',
  'common/JackMidiReceiveQueue.cpp',
  'common/JackMidiSendQueue.cpp',
  'common/JackMidiUtil.cpp',
  'common/JackMidiWriteQueue.cpp',
  'common/JackMetadata.cpp']
src_jack_server_posix = [
    'posix/JackSocketServerChannel.cpp',
    'posix/JackSocketNotifyChannel.cpp',
    'posix/JackSocketServerNotifyChannel.cpp',
    'posix/JackNetUnixSocket.cpp']
src_jack_server_windows = [
  'windows/JackMMCSS.cpp',
  'windows/JackWinNamedPipe.cpp',
  'windows/JackWinNamedPipeServerChannel.cpp',
  'windows/JackWinNamedPipeServerNotifyChannel.cpp',
  'windows/JackWinNamedPipeNotifyChannel.cpp',
  'windows/JackNetWinSocket.cpp']

# sources for libjacknet
src_jack_net = [
  'common/JackNetAPI.cpp',
  'common/JackNetInterface.cpp',
  'common/JackNetTool.cpp',
  'common/JackException.cpp',
  'common/JackAudioAdapterInterface.cpp',
  'common/JackLibSampleRateResampler.cpp',
  'common/JackResampler.cpp',
  'common/JackGlobals.cpp',
  'common/ringbuffer.c']
src_jack_net_posix = [
  'posix/JackNetUnixSocket.cpp',
  'posix/JackPosixThread.cpp',
  'posix/JackPosixMutex.cpp']

# sources for jack/netmanager
src_netmanager = [
  'common/JackNetManager.cpp',
  'common/JackDebugClient.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackInternalClient.cpp',
  'common/JackMessageBuffer.cpp',
  'common/JackTools.cpp',
  'common/JackServerAPI.cpp'
  ]
src_netmanager_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp'
  ]

# sources for jack/profiler
src_profiler = [
  'common/JackProfiler.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
  ]
src_profiler_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
  ]

# sources for jack/netadapter
src_netadapter = [
  'common/JackNetAdapter.cpp',
  'common/JackAudioAdapter.cpp',
  'common/JackAudioAdapterInterface.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackLibSampleRateResampler.cpp',
  'common/JackMessageBuffer.cpp',
  'common/JackResampler.cpp',
]
src_netadapter_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/audioadapter
src_audioadapter = [
  'common/JackAudioAdapter.cpp',
  'common/JackAudioAdapterInterface.cpp',
  'common/JackAudioAdapterFactory.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackLibSampleRateResampler.cpp',
  'common/JackMessageBuffer.cpp',
  'common/JackResampler.cpp',
]
src_audioadapter_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# dependencies for libjack
deps_jack_client = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjackserver
deps_jack_server = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjacknet
deps_jack_net = [dep_threads, dep_celt, dep_opus, dep_samplerate]

# dependencies for jack/netmanager
deps_netmanager = [dep_threads]

# dependencies for jack/profiler
deps_profiler = [dep_threads]

# dependencies for jack/netadapter
deps_netadapter = [dep_threads, dep_samplerate]

# dependencies for jack/audioadapter
deps_audioadapter = [dep_threads, dep_samplerate]

# platform specific overrides for sources, includes, dependencies and linker flags
if os == 'linux'
  src_common_linux = [
    'common/JackDebugClient.cpp',
    'common/timestamps.c',
    'common/promiscuous.c',
    'posix/JackPosixThread.cpp',
    'posix/JackPosixProcessSync.cpp',
    'posix/JackPosixMutex.cpp',
    'posix/JackSocket.cpp',
    'linux/JackLinuxFutex.cpp',
    'linux/JackLinuxTime.c']
  # libjack
  src_jack_client += src_common + src_common_linux + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_jack_client += [lib_rt, lib_dl]
  link_flags_jack_client = []
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_linux + src_jack_server_posix
  include_jack_server = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_jack_server += [lib_rt, lib_dl]
  link_flags_jack_server = []
  symbol_visibility_jack_server = 'hidden'
  # libjacknet
  src_jack_net_linux = ['linux/JackLinuxTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_linux
  include_jack_net = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_jack_net += [lib_rt]
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'hidden'
  # jack/netmanager
  src_netmanager_linux = ['linux/JackLinuxTime.c']
  src_netmanager += src_netmanager_posix + src_netmanager_linux
  include_netmanager = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_netmanager += [lib_dl, lib_rt]
  link_flags_netmanager = []
  symbol_visibility_netmanager = 'hidden'
  # jack/profiler
  src_profiler_linux = ['linux/JackLinuxTime.c']
  src_profiler += src_profiler_posix + src_profiler_linux
  include_profiler = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  link_flags_profiler = []
  symbol_visibility_profiler = 'hidden'
  # jack/netadapter
  src_netadapter_linux = ['linux/JackLinuxTime.c']
  src_netadapter += src_netadapter_posix + src_netadapter_linux
  include_netadapter = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  link_flags_netadapter = []
  symbol_visibility_netadapter = 'hidden'
  # jack/audioadapter
  if dep_alsa.found()
    src_audioadapter_linux = [
      'linux/alsa/JackAlsaAdapter.cpp',
      'linux/JackLinuxTime.c',
    ]
    src_audioadapter += src_audioadapter_posix + src_audioadapter_linux
    include_audioadapter = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
    deps_audioadapter += [dep_alsa]
    link_flags_audioadapter = []
    symbol_visibility_audioadapter = 'hidden'
  endif
endif

if os == 'sunos'
  src_common_sunos += [
    'common/JackDebugClient.cpp',
    'common/timestamps.c',
    'common/promiscuous.c',
    'posix/JackPosixThread.cpp',
    'posix/JackFifo.cpp',
    'posix/JackPosixProcessSync.cpp',
    'posix/JackPosixMutex.cpp',
    'posix/JackSocket.cpp',
    'solaris/JackSolarisTime.c']
  # libjack
  src_jack_client += src_common + src_common_sunos + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  deps_jack_client += [lib_rt]
  link_flags_jack_client = ['-lnsl', '-lsocket']
  symbol_visibility_jack_client = 'default'
  # libjackserver
  src_jack_server += src_common + src_common_sunos + src_jack_server_posix
  include_jack_server = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  deps_jack_server += [lib_rt]
  link_flags_jack_server = ['-lnsl', '-lsocket']
  symbol_visibility_jack_server = 'default'
  # libjacknet
  src_jack_net_sunos = ['solaris/JackSolarisTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_sunos
  include_jack_server = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  deps_jack_net += [lib_rt]
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'hidden'
  # netmanager
  src_netmanager_sunos = ['solaris/JackSolarisTime.c']
  src_netmanager += src_netmanager_posix + src_netmanager_sunos
  include_netmanager = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  deps_netmanager = [dep_threads, lib_dl, lib_rt]
  link_flags_netmanager = []
  symbol_visibility_netmanager = 'hidden'
  # jack/profiler
  src_profiler_sunos = ['solaris/JackSolarisTime.c']
  src_profiler += src_profiler_posix + src_profiler_sunos
  include_profiler = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  link_flags_profiler = []
  symbol_visibility_profiler = 'hidden'
  # jack/netadapter
  src_netadapter_sunos = ['solaris/JackSolarisTime.c']
  src_netadapter += src_netadapter_posix + src_netadapter_sunos
  include_netadapter = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  link_flags_netadapter = []
  symbol_visibility_netadapter = 'hidden'
  # jack/audioadapter
  if dep_oss.found()
    src_audioadapter_sunos = [
      'common/memops.c',
      'solaris/oss/JackOSSAdapter.cpp',
    ]
    src_audioadapter += src_audioadapter_posix + src_audioadapter_sunos
    include_audioadapter = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
    deps_audioadapter += [dep_oss]
    link_flags_audioadapter = []
    symbol_visibility_audioadapter = 'hidden'
  endif
endif

if os == 'darwin'
  src_common_darwin += [
    'common/JackDebugClient.cpp',
    'common/timestamps.c',
    'common/promiscuous.c',
    'posix/JackPosixProcessSync.cpp',
    'posix/JackPosixThread.cpp',
    'posix/JackPosixMutex.cpp',
    'macosx/JackMachThread.mm',
    # 'macosx/JackMachSemaphore.mm',
    'posix/JackPosixSemaphore.cpp',
    'posix/JackSocket.cpp',
    'macosx/JackMachTime.c']
  # libjack
  src_jack_client += src_common + src_common_darwin + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'macosx/RPC', 'posix')
  deps_jack_client += [dep_accelerate, dep_coreaudio]
  link_flags_jack_client = ['-single_module']
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_darwin + src_jack_server_posix
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'macosx/RPC', 'posix')
  deps_jack_server += [dep_accelerate, dep_coreaudio, dep_corefoundation]
  link_flags_jack_server = ['-single_module']
  symbol_visibility_jack_server = 'hidden'
  # libjacknet
  src_jack_net_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_darwin
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'macosx/RPC', 'posix')
  deps_jack_net += [dep_coreaudio]
  link_flags_jack_net = ['-single_module']
  symbol_visibility_jack_net = 'hidden'
  # jack/netmanager
  src_netmanager_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_netmanager += src_netmanager_posix + src_netmanager_darwin
  include_netmanager = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_netmanager += [dep_coreaudio]
  link_flags_netmanager = []
  symbol_visibility_netmanager = 'hidden'
  # jack/profiler
  src_profiler_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_profiler += src_profiler_posix + src_profiler_darwin
  include_profiler = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_profiler += [dep_coreaudio]
  link_flags_profiler = []
  symbol_visibility_profiler = 'hidden'
  # jack/netadapter
  src_netadapter_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_netadapter += src_netadapter_posix + src_netadapter_darwin
  include_netadapter = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  link_flags_netadapter = []
  symbol_visibility_netadapter = 'hidden'
  # jack/audioadapter
  src_audioadapter_darwin = ['macosx/coreaudio/JackCoreAudioAdapter.mm']
  src_audioadapter += src_audioadapter_posix + src_audioadapter_darwin
  include_audioadapter = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_audioadapter += [dep_coreaudio, dep_audiounit, dep_audiotoolbox, dep_coreservices]
  link_flags_audioadapter = []
  symbol_visibility_audioadapter = 'hidden'
endif

if os == 'windows'
  src_common_windows += [
    'windows/JackWinMutex.cpp',
    'windows/JackWinProcessSync.cpp',
    'windows/JackWinSemaphore.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackWinTime.c']
  # libjack
  src_jack_client += src_common + src_common_windows + src_jack_client_windows
  include_jack_client = include_directories('.', 'common/jack', 'common', 'windows')
  deps_jack_client += [dep_regex, dep_ws2_32, dep_psapi, dep_winmm]
  link_flags_jack_client = []
  symbol_visibility_jack_client = 'default'
  # libjackserver
  src_jack_server += src_common + src_common_windows + src_jack_server_windows
  include_jack_server = include_directories('.', 'common/jack', 'common', 'windows')
  deps_jack_server += [dep_regex, dep_ws2_32, dep_psapi, dep_winmm]
  link_flags_jack_server = []
  symbol_visibility_jack_server = 'default'
  # libjacknet
  src_jack_net_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c']
  src_jack_net += src_jack_net_windows
  include_jack_server = include_directories('.', 'common/jack', 'common', 'windows')
  deps_jack_net += [dep_winmm, dep_ws2_32]
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/netmanager
  src_netmanager_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c']
  src_netmanager += src_netmanager_windows
  include_netmanager = include_directories('.', 'common/jack', 'common', 'windows', 'windows/portaudio')
  deps_netmanager += [dep_portaudio]
  link_flags_netmanager = []
  symbol_visibility_netmanager = 'default'
  # jack/profiler
  src_profiler_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c']
  src_profiler += src_profiler_windows
  include_profiler = include_directories('.', 'common/jack', 'common', 'windows', 'windows/portaudio')
  deps_profiler += [dep_portaudio]
  link_flags_profiler = []
  symbol_visibility_profiler = 'default'
  # jack/netadapter
  src_netadapter_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c']
  src_netadapter += src_netadapter_windows
  include_netadapter = include_directories('.', 'common/jack', 'common', 'windows', 'windows/portaudio')
  deps_profiler += [dep_portaudio]
  link_flags_netadapter = []
  symbol_visibility_netadapter = 'default'
  # jack/audioadapter
  src_audioadapter_windows = [
    'windows/portaudio/JackPortAudioAdapter.cpp',
    'windows/portaudio/JackPortAudioDevices.cpp'
    ]
  src_audioadapter += src_audioadapter_posix + src_audioadapter_windows
  include_audioadapter = include_directories('.', 'common/jack', 'common', 'windows', 'windows/portaudio')
  deps_audioadapter += [dep_portaudio]
  link_flags_audioadapter = []
  symbol_visibility_audioadapter = 'default'
endif

# libraries
lib_jack = library('jack',
        version: jack_api_version,
        sources: src_jack_client,
        include_directories: include_jack_client,
        dependencies: deps_jack_client,
        link_args: link_flags_jack_client,
        gnu_symbol_visibility: symbol_visibility_jack_client)
lib_jackserver = library('jackserver',
        cpp_args: ['-DSERVER_SIDE'],
        version: jack_api_version,
        sources: src_jack_server,
        include_directories: include_jack_server,
        dependencies: deps_jack_server,
        link_args: link_flags_jack_server,
        gnu_symbol_visibility: symbol_visibility_jack_server)
lib_jacknet = library('jacknet',
        cpp_args: ['-DSERVER_SIDE'],
        version: jack_api_version,
        sources: src_jack_net,
        include_directories: include_jack_net,
        dependencies: deps_jack_net,
        link_args: link_flags_jack_net,
        gnu_symbol_visibility: symbol_visibility_jack_net)
lib_netmanager = library('netmanager',
        name_prefix: '',
        cpp_args: ['-DSERVER_SIDE'],
        sources: src_netmanager,
        include_directories: include_netmanager,
        dependencies: deps_netmanager,
        link_with: [lib_jackserver],
        link_args: link_flags_netmanager,
        gnu_symbol_visibility: symbol_visibility_netmanager)
lib_profiler = library('profiler',
        name_prefix: '',
        cpp_args: ['-DSERVER_SIDE'],
        sources: src_profiler,
        include_directories: include_profiler,
        dependencies: deps_profiler,
        link_with: [lib_jackserver],
        link_args: link_flags_profiler,
        gnu_symbol_visibility: symbol_visibility_profiler)
lib_netadapter = library('netadapter',
        name_prefix: '',
        cpp_args: ['-DSERVER_SIDE'],
        sources: src_netadapter,
        include_directories: include_netadapter,
        dependencies: deps_netadapter,
        link_with: [lib_jackserver],
        link_args: link_flags_netadapter,
        gnu_symbol_visibility: symbol_visibility_netadapter)
lib_audioadapter = library('audioadapter',
        name_prefix: '',
        cpp_args: ['-DSERVER_SIDE'],
        sources: src_audioadapter,
        include_directories: include_audioadapter,
        dependencies: deps_audioadapter,
        link_with: [lib_jackserver],
        link_args: link_flags_audioadapter,
        gnu_symbol_visibility: symbol_visibility_audioadapter)

# headers
headers_jack = [
  'control.h',
  'intclient.h',
  'jack.h',
  'jslist.h',
  'metadata.h',
  'midiport.h',
  'net.h',
  'ringbuffer.h',
  'session.h',
  'statistics.h',
  'systemdeps.h',
  'thread.h',
  'transport.h',
  'types.h',
  'uuid.h',
  'weakjack.h',
  'weakmacros.h',
]

foreach header : headers_jack
  configure_file(
    input: join_paths('common/jack/', header),
    output: header,
    copy: true,
    install: true,
    install_dir: join_paths(get_option('prefix'), get_option('includedir'))
  )
endforeach

# pkgconfig
pkg = import('pkgconfig')
pkg.generate(lib_jack,
  filebase: 'jack',
  description: 'The Jack Audio Connection Kit: A low-latency synchronous callback-based media server',
  url: 'http://jackaudio.org',
  variables: ['server_libs=-L${prefix}/${libdir} -ljackserver']
)

