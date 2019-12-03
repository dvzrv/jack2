project(
  'jack2',
  ['c', 'cpp'],
  default_options: [
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
# TODO: turn into cpp_args
conf_data.set('addon_dir', join_paths(get_option('prefix'), get_option('libdir'), '/jack'))
config_header = configure_file(
  input: 'common/config.h.in',
  output: 'config.h',
  configuration: conf_data)

# c library dependencies
lib_dl = meson.get_compiler('c').find_library('dl')
lib_m = meson.get_compiler('c').find_library('m')
lib_db = meson.get_compiler('c').find_library('db')
lib_c = meson.get_compiler('c').find_library('libc')
lib_stdcpp = meson.get_compiler('cpp').find_library('stdc++')

if os in ['linux', 'sunos']
  lib_rt = meson.get_compiler('c').find_library('rt')
endif

# package dependencies
if os == 'sunos'
  dep_oss = dependency('oss')
endif

if os == 'darwin'
  add_language('objc')
  dep_accelerate = dependency('Accelerate', method: 'extraframework', disabler: true)
  dep_audiounit = dependency('AudioUnit', method: 'extraframework', disabler: true)
  dep_audiotoolbox = dependency('AudioToolbox', method: 'extraframework', disabler: true)
  dep_coreaudio = dependency('CoreAudio', method: 'extraframework', disabler: true)
  dep_corefoundation = dependency('CoreFoundation', method: 'extraframework', disabler: true)
  dep_coreservices = dependency('CoreServices', method: 'extraframework', disabler: true)
endif

if os == 'windows'
  dep_regex = dependency('REGEX')
  dep_ws2_32 = dependency('WS2_32')
  dep_psapi = dependency('PSAPI')
  dep_portaudio = dependency('portaudio')
  dep_winmm = dependency('WINMM')
endif

dep_alsa = dependency('alsa', version: '>=1.0.18', required: get_option('alsa'), disabler: true)
dep_dbus = dependency('dbus-1', version: '>=1.0.0', required: get_option('dbus'), disabler: true)
dep_ffado = dependency('libffado', version: '>=1.999.17', required: get_option('ffado'), disabler: true)
dep_iio = dependency('libiio', version: '>=0.16', required: get_option('iio'), disabler: true)
dep_gtkiostream = dependency('gtkIOStream', version: '>=1.7.0', required: get_option('iio'), disabler: true)
dep_systemd = dependency('systemd', required: get_option('systemd'), disabler: true)
dep_celt = dependency('celt', version: '>=0.11.0')
dep_opus = dependency('opus', version: '>=0.9.0')
dep_readline = dependency('readline')
dep_samplerate = dependency('samplerate')
dep_sndfile = dependency('sndfile')
dep_threads = dependency('threads')

doxygen = find_program('doxygen', required : get_option('doxygen'), disabler: true)

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
src_jacknet = [
  'common/JackNetAPI.cpp',
  'common/JackNetInterface.cpp',
  'common/JackNetTool.cpp',
  'common/JackException.cpp',
  'common/JackAudioAdapterInterface.cpp',
  'common/JackLibSampleRateResampler.cpp',
  'common/JackResampler.cpp',
  'common/JackGlobals.cpp',
  'common/ringbuffer.c']
src_jacknet_posix = [
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

# sources for jackd
src_jackd = [
  'common/Jackdmp.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jackd_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_dummy
src_jack_dummy = [
  'common/JackDummyDriver.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_dummy_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_loopback
src_jack_loopback = [
  'common/JackLoopbackDriver.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_loopback_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_net
src_jack_net = [
  'common/JackNetDriver.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_net_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_netone
src_jack_netone = [
  'common/JackNetOneDriver.cpp',
  'common/netjack.c',
  'common/netjack_packet.c',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_netone_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_proxy
src_jack_proxy = [
  'common/JackProxyDriver.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_proxy_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_alsa
src_jack_alsa = [
  'common/memops.c',
  'linux/alsa/JackAlsaDriver.cpp',
  'linux/alsa/alsa_rawmidi.c',
  'linux/alsa/alsa_seqmidi.c',
  'linux/alsa/alsa_midi_jackmp.cpp',
  'linux/alsa/generic_hw.c',
  'linux/alsa/hdsp.c',
  'linux/alsa/alsa_driver.c',
  'linux/alsa/hammerfall.c',
  'linux/alsa/ice1712.c',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_alsa_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_alsarawmidi
src_jack_alsarawmidi = [
  'linux/alsarawmidi/JackALSARawMidiDriver.cpp',
  'linux/alsarawmidi/JackALSARawMidiInputPort.cpp',
  'linux/alsarawmidi/JackALSARawMidiOutputPort.cpp',
  'linux/alsarawmidi/JackALSARawMidiPort.cpp',
  'linux/alsarawmidi/JackALSARawMidiReceiveQueue.cpp',
  'linux/alsarawmidi/JackALSARawMidiSendQueue.cpp',
  'linux/alsarawmidi/JackALSARawMidiUtil.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_alsarawmidi_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_firewire
src_jack_firewire = [
  'linux/firewire/JackFFADODriver.cpp',
  'linux/firewire/JackFFADOMidiInputPort.cpp',
  'linux/firewire/JackFFADOMidiOutputPort.cpp',
  'linux/firewire/JackFFADOMidiReceiveQueue.cpp',
  'linux/firewire/JackFFADOMidiSendQueue.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_firewire_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# sources for jack/jack_iio
src_jack_iio = [
  'linux/iio/JackIIODriver.cpp',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jack_iio_posix = [
  'posix/JackPosixProcessSync.cpp',
  'posix/JackPosixThread.cpp',
]

# dependencies for libjack
deps_jack_client = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjackserver
deps_jack_server = [dep_threads, dep_celt, dep_opus, lib_db]

# dependencies for libjacknet
deps_jacknet = [dep_threads, dep_celt, dep_opus, dep_samplerate]

# dependencies for jack/netmanager
deps_netmanager = [dep_threads]

# dependencies for jack/profiler
deps_profiler = [dep_threads]

# dependencies for jack/netadapter
deps_netadapter = [dep_threads, dep_samplerate]

# dependencies for jack/audioadapter
deps_audioadapter = [dep_threads, dep_samplerate]

# dependencies for jackd
deps_jackd = [dep_threads]

# dependencies for jack/jack_dummy
deps_jack_dummy = [dep_threads]

# dependencies for jack/jack_loopback
deps_jack_loopback = [dep_threads]

# dependencies for jack/jack_net
deps_jack_net = [dep_threads]

# dependencies for jack/jack_netone
deps_jack_netone = [dep_celt, dep_samplerate, dep_threads]

# dependencies for jack/jack_proxy
deps_jack_proxy = [dep_threads]

# dependencies for jack/jack_alsa
deps_jack_alsa = [dep_threads]

# dependencies for jack/jack_alsarawmidi
deps_jack_alsarawmidi = [dep_threads]

# dependencies for jack/jack_firewire
deps_jack_firewire = [dep_threads]

# dependencies for jack/jack_iio
deps_jack_iio = [dep_threads]

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
  cpp_args_jack_client = []
  src_jack_client += src_common + src_common_linux + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  if dep_dbus.found() and get_option('autostart-method') == 'dbus'
    message('Automatic starting through clients is done with D-Bus activation.')
    cpp_args_jack_client += ['-DUSE_LIBDBUS_AUTOLAUNCH=1']
    deps_jack_client += [dep_dbus]
  endif
  if get_option('autostart-method') == 'classic'
    message('Automatic starting through clients is done with daemon activation.')
    cpp_args_jack_client += [
      '-DUSE_CLASSIC_AUTOLAUNCH=1',
      '-DJACK_LOCATION="' + join_paths(get_option('prefix'), get_option('bindir')) + '"'
    ]
  endif
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
  src_jacknet_linux = ['linux/JackLinuxTime.c']
  src_jacknet += src_jacknet_posix + src_jacknet_linux
  include_jacknet = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_jacknet += [lib_rt]
  link_flags_jacknet = []
  symbol_visibility_jacknet = 'hidden'
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
  # jackd
  src_jackd_linux = ['linux/JackLinuxTime.c']
  src_jackd += src_jackd_posix + src_jackd_linux
  include_jackd = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jackd += [lib_dl, lib_m, lib_rt, lib_stdcpp]
  if dep_systemd.found()
    message('Compiling jackd with systemd support.')
    deps_jackd += [dep_systemd]
  endif
  if dep_dbus.found()
    message('Compiling jackd with D-Bus support.')
    src_jackd_linux_dbus = [
      'dbus/audio_reserve.c',
      'dbus/reserve.c',
    ]
    src_jackd += src_jackd_linux_dbus
    deps_jackd += [dep_dbus]
  endif
  link_flags_jackd = []
  # jack/jack_dummy
  src_jack_dummy_linux = ['linux/JackLinuxTime.c']
  src_jack_dummy += src_jack_dummy_posix + src_jack_dummy_linux
  include_jack_dummy = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  link_flags_jack_dummy = []
  symbol_visibility_jack_dummy = 'default'
  # jack/jack_loopback
  src_jack_loopback_linux = ['linux/JackLinuxTime.c']
  src_jack_loopback += src_jack_loopback_posix + src_jack_loopback_linux
  include_jack_loopback = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  link_flags_jack_loopback = []
  symbol_visibility_jack_loopback = 'default'
  # jack/jack_net
  src_jack_net_linux = ['linux/JackLinuxTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_linux
  include_jack_net = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/jack_netone
  src_jack_netone_linux = ['linux/JackLinuxTime.c']
  src_jack_netone += src_jack_netone_posix + src_jack_netone_linux
  include_jack_netone = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  link_flags_jack_netone = []
  symbol_visibility_jack_netone = 'default'
  # jack/jack_proxy
  src_jack_proxy_linux = ['linux/JackLinuxTime.c']
  src_jack_proxy += src_jack_proxy_posix + src_jack_proxy_linux
  include_jack_proxy = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_proxy += [lib_dl]
  link_flags_jack_proxy = []
  symbol_visibility_jack_proxy = 'default'
  if dep_alsa.found()
    message('Building ALSA drivers.')
    # jack/jack_alsa
    src_jack_alsa_linux = ['linux/JackLinuxTime.c']
    src_jack_alsa += src_jack_alsa_posix + src_jack_alsa_linux
    cpp_args_jack_alsa = ['-DJACKMP=1', '-DSERVER_SIDE']
    c_args_jack_alsa = ['-DJACKMP=1', '-DSERVER_SIDE']
    include_jack_alsa = include_directories('.', 'common', 'common/jack', 'linux/alsa', 'linux', 'posix')
    deps_jack_alsa += [dep_alsa]
    link_flags_jack_alsa = []
    symbol_visibility_jack_alsa = 'default'
    # jack/jack_alsarawmidi
    src_jack_alsarawmidi_linux = ['linux/JackLinuxTime.c']
    src_jack_alsarawmidi += src_jack_alsarawmidi_posix + src_jack_alsarawmidi_linux
    cpp_args_jack_alsarawmidi = ['-DSERVER_SIDE']
    include_jack_alsarawmidi = include_directories('.', 'common', 'common/jack', 'linux/alsa', 'linux', 'posix')
    deps_jack_alsarawmidi += [dep_alsa]
    link_flags_jack_alsarawmidi = []
    symbol_visibility_jack_alsarawmidi = 'default'
  endif
  if dep_ffado.found()
    message('Building FFADO driver.')
    # jack/jack_firewire
    src_jack_firewire_linux = ['linux/JackLinuxTime.c']
    src_jack_firewire += src_jack_firewire_posix + src_jack_firewire_linux
    cpp_args_jack_firewire = ['-DSERVER_SIDE']
    include_jack_firewire = include_directories('.', 'common', 'common/jack', 'linux/firewire', 'linux', 'posix')
    deps_jack_firewire += [dep_ffado]
    link_flags_jack_firewire = []
    symbol_visibility_jack_firewire = 'default'
  endif
  if dep_iio.found() and dep_gtkiostream.found()
    message('Building IIO driver.')
    # jack/jack_iio
    src_jack_iio_linux = ['linux/JackLinuxTime.c']
    src_jack_iio += src_jack_iio_posix + src_jack_iio_linux
    cpp_args_jack_iio = ['-DSERVER_SIDE']
    include_jack_iio = include_directories('.', 'common', 'common/jack', 'linux/iio', 'linux', 'posix')
    deps_jack_iio += [dep_iio, dep_gtkiostream]
    link_flags_jack_iio = []
    symbol_visibility_jack_iio = 'default'
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
    'solaris/JackSolarisTime.c',
  ]
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
  src_jacknet_sunos = ['solaris/JackSolarisTime.c']
  src_jacknet += src_jacknet_posix + src_jacknet_sunos
  include_jack_server = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
  deps_jacknet += [lib_rt]
  link_flags_jacknet = []
  symbol_visibility_jacknet = 'hidden'
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
  # jackd
  src_jackd_sunos = ['solaris/JackSolarisTime.c']
  src_jackd += src_jackd_posix + src_jackd_sunos
  include_jackd = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_jackd += [lib_dl]
  link_flags_jackd = []
  # jack/jack_dummy
  src_jack_dummy_sunos = ['solaris/JackSolarisTime.c']
  src_jack_dummy += src_jack_dummy_posix + src_jack_dummy_sunos
  include_jack_dummy = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  link_flags_jack_dummy = []
  symbol_visibility_jack_dummy = 'default'
  # jack/jack_loopback
  src_jack_loopback_sunos = ['solaris/JackSolarisTime.c']
  src_jack_loopback += src_jack_loopback_posix + src_jack_loopback_sunos
  include_jack_loopback = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  link_flags_jack_loopback = []
  symbol_visibility_jack_loopback = 'default'
  # jack/jack_net
  src_jack_net_sunos = ['solaris/JackSolarisTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_sunos
  include_jack_net = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/jack_netone
  src_jack_net_sunos = ['solaris/JackSolarisTime.c']
  src_jack_netone += src_jack_netone_posix + src_jack_netone_sunos
  include_jack_netone = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  link_flags_jack_netone = []
  symbol_visibility_jack_netone = 'default'
  # jack/jack_proxy
  src_jack_proxy_sunos = ['solaris/JackSolarisTime.c']
  src_jack_proxy += src_jack_proxy_posix + src_jack_proxy_sunos
  include_jack_proxy = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_jack_proxy += [lib_dl]
  link_flags_jack_proxy = []
  symbol_visibility_jack_proxy = 'default'
endif

if os == 'darwin'
  src_common_darwin = [
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
    'macosx/JackMachTime.c',
  ]
  # libjack
  src_jack_client += src_common + src_common_darwin + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jack_client += [dep_accelerate, dep_coreaudio]
  link_flags_jack_client = ['-single_module']
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_darwin + src_jack_server_posix
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jack_server += [dep_accelerate, dep_coreaudio, dep_corefoundation]
  link_flags_jack_server = ['-single_module']
  symbol_visibility_jack_server = 'hidden'
  # libjacknet
  src_jacknet_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jacknet += src_jacknet_posix + src_jacknet_darwin
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jacknet += [dep_coreaudio]
  link_flags_jacknet = ['-single_module']
  symbol_visibility_jacknet = 'hidden'
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
  # jackd
  src_jackd_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jackd += src_jackd_posix + src_jackd_darwin
  include_jackd = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jackd += [lib_dl]
  link_flags_jackd = []
  # jack/jack_dummy
  src_jack_dummy_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_dummy += src_jack_dummy_posix + src_jack_dummy_darwin
  include_jack_dummy = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  link_flags_jack_dummy = []
  symbol_visibility_jack_dummy = 'default'
  # jack/jack_loopback
  src_jack_loopback_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_loopback += src_jack_loopback_posix + src_jack_loopback_darwin
  include_jack_loopback = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  link_flags_jack_loopback = []
  symbol_visibility_jack_loopback = 'default'
  # jack/jack_net
  src_jack_net_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_darwin
  include_jack_net = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/jack_netone
  src_jack_netone_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_netone += src_jack_netone_posix + src_jack_netone_darwin
  include_jack_netone = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  link_flags_jack_netone = []
  symbol_visibility_jack_netone = 'default'
  # jack/jack_proxy
  src_jack_proxy_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_proxy += src_jack_proxy_posix + src_jack_proxy_sunos
  include_jack_proxy = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_proxy += [lib_dl]
  link_flags_jack_proxy = []
  symbol_visibility_jack_proxy = 'default'
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
  src_jacknet_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c']
  src_jacknet += src_jacknet_windows
  include_jack_server = include_directories('.', 'common/jack', 'common', 'windows')
  deps_jacknet += [dep_winmm, dep_ws2_32]
  link_flags_jacknet = []
  symbol_visibility_jacknet = 'default'
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
    'windows/portaudio/JackPortAudioDevices.cpp',
  ]
  src_audioadapter += src_audioadapter_posix + src_audioadapter_windows
  include_audioadapter = include_directories('.', 'common/jack', 'common', 'windows', 'windows/portaudio')
  deps_audioadapter += [dep_portaudio]
  link_flags_audioadapter = []
  symbol_visibility_audioadapter = 'default'
  # jackd
  src_jackd_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jackd += src_jackd_windows
  include_jackd = include_directories('.', 'common/jack', 'common', 'windows',)
  link_flags_jackd = []
  # jack/jack_dummy
  src_jack_dummy_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jack_dummy += src_jack_dummy_posix + src_jack_dummy_windows
  include_jack_dummy = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_dummy = []
  symbol_visibility_jack_dummy = 'default'
  # jack/jack_loopback
  src_jack_loopback_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jack_loopback += src_jack_loopback_posix + src_jack_loopback_windows
  include_jack_loopback = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_loopback = []
  symbol_visibility_jack_loopback = 'default'
  # jack/jack_net
  src_jack_net_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jack_net += src_jack_net_posix + src_jack_net_windows
  include_jack_net = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/jack_netone
  src_jack_netone_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jack_netone += src_jack_netone_posix + src_jack_netone_windows
  include_jack_netone = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_netone = []
  symbol_visibility_jack_netone = 'default'
  # jack/jack_proxy
  src_jack_netone_windows = [
    'windows/JackNetWinSocket.cpp',
    'windows/JackWinThread.cpp',
    'windows/JackMMCSS.cpp',
    'windows/JackWinTime.c',
  ]
  src_jack_proxy += src_jack_proxy_posix + src_jack_proxy_windows
  include_jack_proxy = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_proxy = []
  symbol_visibility_jack_proxy = 'default'
endif

# libraries
lib_jack = library(
  'jack',
  cpp_args: cpp_args_jack_client,
  version: jack_api_version,
  sources: src_jack_client,
  include_directories: include_jack_client,
  dependencies: deps_jack_client,
  link_args: link_flags_jack_client,
  gnu_symbol_visibility: symbol_visibility_jack_client,
  install: true
)
lib_jackserver = library(
  'jackserver',
  cpp_args: ['-DSERVER_SIDE'],
  version: jack_api_version,
  sources: src_jack_server,
  include_directories: include_jack_server,
  dependencies: deps_jack_server,
  link_args: link_flags_jack_server,
  gnu_symbol_visibility: symbol_visibility_jack_server,
  install: true
)
lib_jacknet = library(
  'jacknet',
  cpp_args: ['-DSERVER_SIDE'],
  version: jack_api_version,
  sources: src_jacknet,
  include_directories: include_jacknet,
  dependencies: deps_jacknet,
  link_args: link_flags_jacknet,
  gnu_symbol_visibility: symbol_visibility_jacknet,
  install: true
)
lib_netmanager = library(
  'netmanager',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_netmanager,
  include_directories: include_netmanager,
  dependencies: deps_netmanager,
  link_with: [lib_jackserver],
  link_args: link_flags_netmanager,
  gnu_symbol_visibility: symbol_visibility_netmanager,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_profiler = library(
  'profiler',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_profiler,
  include_directories: include_profiler,
  dependencies: deps_profiler,
  link_with: [lib_jackserver],
  link_args: link_flags_profiler,
  gnu_symbol_visibility: symbol_visibility_profiler,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_netadapter = library(
  'netadapter',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_netadapter,
  include_directories: include_netadapter,
  dependencies: deps_netadapter,
  link_with: [lib_jackserver],
  link_args: link_flags_netadapter,
  gnu_symbol_visibility: symbol_visibility_netadapter,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_audioadapter = library(
  'audioadapter',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_audioadapter,
  include_directories: include_audioadapter,
  dependencies: deps_audioadapter,
  link_with: [lib_jackserver],
  link_args: link_flags_audioadapter,
  gnu_symbol_visibility: symbol_visibility_audioadapter,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)

# drivers
lib_jack_dummy = library(
  'jack_dummy',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jack_dummy,
  include_directories: include_jack_dummy,
  dependencies: deps_jack_dummy,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_dummy,
  gnu_symbol_visibility: symbol_visibility_jack_dummy,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_jack_loopback = library(
  'jack_loopback',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jack_loopback,
  include_directories: include_jack_loopback,
  dependencies: deps_jack_loopback,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_loopback,
  gnu_symbol_visibility: symbol_visibility_jack_loopback,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_jack_net = library(
  'jack_net',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jack_net,
  include_directories: include_jack_net,
  dependencies: deps_jack_net,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_net,
  gnu_symbol_visibility: symbol_visibility_jack_net,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_jack_netone = library(
  'jack_netone',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jack_netone,
  include_directories: include_jack_netone,
  dependencies: deps_jack_netone,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_netone,
  gnu_symbol_visibility: symbol_visibility_jack_netone,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_jack_proxy = library(
  'jack_proxy',
  name_prefix: '',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jack_proxy,
  include_directories: include_jack_proxy,
  dependencies: deps_jack_proxy,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_proxy,
  gnu_symbol_visibility: symbol_visibility_jack_proxy,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
if os == 'linux' and dep_alsa.found()
  lib_jack_alsa = library(
    'jack_alsa',
    name_prefix: '',
    cpp_args: cpp_args_jack_alsa,
    c_args: c_args_jack_alsa,
    sources: src_jack_alsa,
    include_directories: include_jack_alsa,
    dependencies: deps_jack_alsa,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_alsa,
    gnu_symbol_visibility: symbol_visibility_jack_alsa,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
  lib_jack_alsarawmidi = library(
    'jack_alsarawmidi',
    name_prefix: '',
    cpp_args: cpp_args_jack_alsarawmidi,
    sources: src_jack_alsarawmidi,
    include_directories: include_jack_alsarawmidi,
    dependencies: deps_jack_alsarawmidi,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_alsarawmidi,
    gnu_symbol_visibility: symbol_visibility_jack_alsarawmidi,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'linux' and dep_ffado.found()
  lib_jack_firewire = library(
    'jack_firewire',
    name_prefix: '',
    cpp_args: cpp_args_jack_firewire,
    sources: src_jack_firewire,
    include_directories: include_jack_firewire,
    dependencies: deps_jack_firewire,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_firewire,
    gnu_symbol_visibility: symbol_visibility_jack_firewire,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'linux' and dep_iio.found() and dep_gtkiostream.found()
  lib_jack_iio = library(
    'jack_iio',
    name_prefix: '',
    cpp_args: cpp_args_jack_iio,
    sources: src_jack_iio,
    include_directories: include_jack_iio,
    dependencies: deps_jack_iio,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_iio,
    gnu_symbol_visibility: symbol_visibility_jack_iio,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif

# headers
install_headers(
  'common/jack/control.h',
  'common/jack/intclient.h',
  'common/jack/jack.h',
  'common/jack/jslist.h',
  'common/jack/metadata.h',
  'common/jack/midiport.h',
  'common/jack/net.h',
  'common/jack/ringbuffer.h',
  'common/jack/session.h',
  'common/jack/statistics.h',
  'common/jack/systemdeps.h',
  'common/jack/thread.h',
  'common/jack/transport.h',
  'common/jack/types.h',
  'common/jack/uuid.h',
  'common/jack/weakjack.h',
  'common/jack/weakmacros.h',
  subdir: 'jack')

# pkgconfig
pkg = import('pkgconfig')
pkg.generate(lib_jack,
  filebase: 'jack',
  description: 'The Jack Audio Connection Kit: A low-latency synchronous callback-based media server',
  url: 'http://jackaudio.org',
  variables: ['server_libs=-L${prefix}/${libdir} -ljackserver']
)

# executables
exe_jackd = executable(
  'jackd',
  cpp_args: ['-DSERVER_SIDE'],
  sources: src_jackd,
  include_directories: include_jackd,
  dependencies: deps_jackd,
  link_with: [lib_jackserver],
  link_args: link_flags_jackd,
  install: true
)

# system integration
if os == 'linux' and dep_systemd.found()
  config_systemd = configuration_data(
    {'PREFIX': get_option('prefix')}
  )
  configure_file(
    input: 'systemd/jack@.service.in',
    output: 'jack@.service',
    install: true,
    install_dir: dep_systemd.get_pkgconfig_variable('systemduserunitdir'),
    configuration: config_systemd
  )
  configure_file(
    input: 'systemd/example.conf',
    output: 'example.conf',
    copy: true,
    install: true,
    install_dir: join_paths(get_option('sysconfdir'), 'jack')
  )
endif
