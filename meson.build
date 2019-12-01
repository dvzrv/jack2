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
conf_data.set('addon_dir', get_option('prefix') + get_option('libdir') + '/jack')
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

if os == 'darwin'
  dep_accelerate = dependency('Accelerate')
  dep_coreaudio = dependency('CoreAudio')
  dep_corefoundation = dependency('CoreFoundation')
endif

if os == 'windows'
  dep_regex = dependency('REGEX')
  dep_ws2_32 = dependency('WS2_32')
  dep_psapi = dependency('PSAPI')
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

# dependencies for libjack
deps_jack_client = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjackserver
deps_jack_server = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjackserver
deps_jack_net = [dep_threads, dep_celt, dep_opus, dep_samplerate]

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
  deps_jack_client += [dep_accelerate, dep_coraudio]
  link_flags_jack_client = ['-single_module']
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_darwin + src_jack_server_posix
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'macosx/RPC', 'posix')
  deps_jack_server += [dep_accelerate, dep_coraudio, dep_corefoundation]
  link_flags_jack_server = ['-single_module']
  symbol_visibility_jack_server = 'hidden'
  # libjacknet
  src_jack_net_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_darwin
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'macosx/RPC', 'posix')
  deps_jack_net += [dep_coraudio]
  link_flags_jack_net = ['-single_module']
  symbol_visibility_jack_net = 'hidden'
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
endif

# libraries
library('jack',
        version: jack_api_version,
        sources: src_jack_client,
        include_directories: include_jack_client,
        dependencies: deps_jack_client,
        link_args: link_flags_jack_client,
        gnu_symbol_visibility: symbol_visibility_jack_client)
library('jackserver',
        version: jack_api_version,
        sources: src_jack_server,
        include_directories: include_jack_server,
        dependencies: deps_jack_server,
        link_args: link_flags_jack_server,
        gnu_symbol_visibility: symbol_visibility_jack_server)
library('jacknet',
        version: jack_api_version,
        sources: src_jack_net,
        include_directories: include_jack_net,
        dependencies: deps_jack_net,
        link_args: link_flags_jack_net,
        gnu_symbol_visibility: symbol_visibility_jack_net)
