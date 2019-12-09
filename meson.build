project(
  'jack2',
  ['c', 'cpp'],
  meson_version: '>=0.50.0',
  license: ['GPL2+', 'LGPL2.1'],
  version: '1.9.14')

jack_api_version = '0.1.0'
os = build_machine.system()
conf_data = configuration_data()
conf_data.set('version', meson.project_version())
conf_data.set('clients', get_option('clients'))
conf_data.set('ports-per-client', get_option('ports-per-client'))
conf_data.set10('profiling', get_option('profiling'))

# languages
if os == 'darwin'
  add_languages('objcpp')
endif

# c library dependencies
lib_dl = meson.get_compiler('c').find_library('dl')
lib_m = meson.get_compiler('c').find_library('m')
if os == 'freebsd'
  lib_db = meson.get_compiler('c').find_library('db-5')
else
  lib_db = meson.get_compiler('c').find_library('db')
endif
lib_stdcpp = meson.get_compiler('cpp').find_library('stdc++')

if os in ['freebsd', 'linux', 'sunos']
  lib_rt = meson.get_compiler('c').find_library('rt')
endif

if os == 'darwin'
  lib_aften = meson.get_compiler('cpp').find_library('aften')
endif

# package dependencies
dep_celt = dependency('celt', version: '>=0.11.0', required: get_option('celt'))
dep_opus = dependency('opus', version: '>=0.9.0')
dep_readline = dependency('readline')
dep_samplerate = dependency('samplerate')
dep_sndfile = dependency('sndfile')
dep_threads = dependency('threads')

if os in ['freebsd', 'sunos']
  dep_oss = dependency('oss', required: get_option('oss'))
endif

if os == 'darwin'
  dep_accelerate = dependency('Accelerate', method: 'extraframework', disabler: true)
  dep_audiounit = dependency('AudioUnit', method: 'extraframework', disabler: true)
  dep_audiotoolbox = dependency('AudioToolbox', method: 'extraframework', disabler: true)
  dep_coreaudio = dependency('CoreAudio', method: 'extraframework', disabler: true)
  dep_corefoundation = dependency('CoreFoundation', method: 'extraframework', disabler: true)
  dep_coremidi = dependency('CoreMidi', method: 'extraframework', disabler: true)
  dep_coreservices = dependency('CoreServices', method: 'extraframework', disabler: true)
  dep_foundation = dependency('Foundation', method: 'extraframework', disabler: true)
endif

if os == 'windows'
  dep_regex = dependency('REGEX')
  dep_ws2_32 = dependency('WS2_32')
  dep_psapi = dependency('PSAPI')
  dep_portaudio = dependency('portaudio', required: get_option('portaudio'))
  dep_winmm = dependency('WINMM', required: get_option('winmme'))
endif

if os == 'linux'
  dep_alsa = dependency('alsa', version: '>=1.0.18', required: get_option('alsa'), disabler: true)
  dep_ffado = dependency('libffado', version: '>=1.999.17', required: get_option('ffado'), disabler: true)
  dep_iio = dependency('libiio', version: '>=0.16', required: get_option('iio'), disabler: true)
  dep_gtkiostream = dependency('gtkIOStream', version: '>=1.7.0', required: get_option('iio'), disabler: true)
  dep_systemd = dependency('systemd', required: get_option('systemd'), disabler: true)
endif

if os in ['darwin', 'freebsd', 'linux', 'sunos']
  dep_dbus = dependency('dbus-1', version: '>=1.0.0', required: get_option('dbus'), disabler: true)
  dep_expat = dependency('expat', required: get_option('dbus'), disabler: true)
endif

prog_doxygen = find_program('doxygen', required : get_option('doxygen'), disabler: true)

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

# sources for jackdbus
src_jackdbus = [
  'dbus/jackdbus.c',
  'dbus/controller.c',
  'dbus/params.c',
  'dbus/controller_iface_configure.c',
  'dbus/controller_iface_control.c',
  'dbus/controller_iface_introspectable.c',
  'dbus/controller_iface_patchbay.c',
  'dbus/controller_iface_session_manager.c',
  'dbus/controller_iface_transport.c',
  'dbus/xml.c',
  'dbus/xml_expat.c',
  #'dbus/xml_nop.c',
  'dbus/xml_write_raw.c',
  'dbus/sigsegv.c',
  'dbus/reserve.c',
  'common/JackError.cpp',
  'common/JackGlobals.cpp',
  'common/JackMessageBuffer.cpp',
]
src_jackdbus_posix = [
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

# sources for jack/jack_boomer
src_jack_boomer = [
  'common/memops.c',
  'solaris/oss/JackBoomerDriver.cpp',
]

# sources for jack/jack_oss
src_jack_oss = [
  'common/memops.c',
  'solaris/oss/JackOSSDriver.cpp',
]

# sources for jack/jack_coreaudio
src_jack_coreaudio = [
  'macosx/coreaudio/JackCoreAudioDriver.mm',
  'common/JackAC3Encoder.cpp'
]

# sources for jack/jack_coremidi
src_jack_coremidi = [
  'macosx/coremidi/JackCoreMidiInputPort.mm',
  'macosx/coremidi/JackCoreMidiOutputPort.mm',
  'macosx/coremidi/JackCoreMidiPhysicalInputPort.mm',
  'macosx/coremidi/JackCoreMidiPhysicalOutputPort.mm',
  'macosx/coremidi/JackCoreMidiVirtualInputPort.mm',
  'macosx/coremidi/JackCoreMidiVirtualOutputPort.mm',
  'macosx/coremidi/JackCoreMidiPort.mm',
  'macosx/coremidi/JackCoreMidiUtil.mm',
  'macosx/coremidi/JackCoreMidiDriver.mm'
]

# sources for jack/jack_portaudio
src_jack_portaudio = [
  'windows/portaudio/JackPortAudioDevices.cpp',
  'windows/portaudio/JackPortAudioDriver.cpp',
]

# sources for jack/jack_winmme
src_jack_winmme = [
  'windows/winmme/JackWinMMEDriver.cpp',
  'windows/winmme/JackWinMMEInputPort.cpp',
  'windows/winmme/JackWinMMEOutputPort.cpp',
  'windows/winmme/JackWinMMEPort.cpp',
]

# sources for jack_freewheel
src_jack_freewheel = [
  'example-clients/freewheel.c',
]

# sources for jack_connect
src_jack_connect = [
  'example-clients/connect.c',
]

# sources for jack_lsp
src_jack_lsp = [
  'example-clients/lsp.c',
]

# sources for jack_metro
src_jack_metro = [
  'example-clients/metro.c',
]

# sources for jack_midiseq
src_jack_midiseq = [
  'example-clients/midiseq.c',
]

# sources for jack_midisine
src_jack_midisine = [
  'example-clients/midisine.c',
]

# sources for jack_showtime
src_jack_showtime = [
  'example-clients/showtime.c',
]

# sources for jack_simple_client
src_jack_simple_client = [
  'example-clients/simple_client.c',
]

# sources for jack_zombie
src_jack_zombie = [
  'example-clients/zombie.c',
]

# sources for jack_load
src_jack_load = [
  'example-clients/ipload.c',
]

# sources for jack_unload
src_jack_unload = [
  'example-clients/ipunload.c',
]

# sources for jack_alias
src_jack_alias = [
  'example-clients/alias.c',
]

# sources for jack_bufsize
src_jack_bufsize = [
  'example-clients/bufsize.c',
]

# sources for jack_wait
src_jack_wait = [
  'example-clients/wait.c',
]

# sources for jack_samplerate
src_jack_samplerate = [
  'example-clients/samplerate.c',
]

# sources for jack_evmon
src_jack_evmon = [
  'example-clients/evmon.c',
]

# sources for jack_monitor_client
src_jack_monitor_client = [
  'example-clients/monitor_client.c',
]

# sources for jack_thru
src_jack_thru = [
  'example-clients/thru_client.c',
]

# sources for jack_cpu_load
src_jack_cpu_load = [
  'example-clients/cpu_load.c',
]

# sources for jack_simple_session_client
src_jack_simple_session_client = [
  'example-clients/simple_session_client.c',
]

# sources for jack_session_notify
src_jack_session_notify = [
  'example-clients/session_notify.c',
]

# sources for jack_server_control
src_jack_server_control = [
  'example-clients/server_control.cpp',
]

# sources for jack_net_slave
src_jack_net_slave = [
  'example-clients/netslave.c',
]

# sources for jack_net_master
src_jack_net_master = [
  'example-clients/netmaster.c',
]

# sources for jack_latent_client
src_jack_latent_client = [
  'example-clients/latent_client.c',
]

# sources for jack_midi_dump
src_jack_midi_dump = [
  'example-clients/midi_dump.c',
]

# sources for jack_midi_latency_test
src_jack_midi_latency_test = [
  'example-clients/midi_latency_test.c',
]

# sources for jack_simdtests
src_jack_simdtests = [
  'example-clients/simdtests.cpp',
]

# sources for jack_property
src_jack_property = [
  'example-clients/property.c',
]

# sources for jack_transport
src_jack_transport = [
  'example-clients/transport.c',
]

# sources for jack_rec
src_jack_rec = [
  'example-clients/capture_client.c',
]

# sources for jack_netsource
src_jack_netsource = [
  'example-clients/netsource.c',
  'common/netjack_packet.c'
]

# sources for alsa_in
src_alsa_in = [
  'example-clients/alsa_in.c',
  'common/memops.c'
]

# sources for alsa_out
src_alsa_out = [
  'example-clients/alsa_out.c',
  'common/memops.c'
]

# sources for inprocess
src_inprocess = ['example-clients/inprocess.c']

# sources for jack_test
src_jack_test = ['tests/test.cpp']

# sources for jack_cpu
src_jack_cpu = ['tests/cpu.c']

# sources for jack_iodelay
src_jack_iodelay = ['tests/iodelay.cpp']

# sources for jack_multiple_metro
src_jack_multiple_metro = ['tests/external_metro.cpp']

# cpp_args
cpp_args_common = [
  '-DCLIENT_NUM=@0@'.format(conf_data.get('clients')),
  '-DPORT_NUM_FOR_CLIENT=@0@'.format(conf_data.get('ports-per-client')),
  '-DJACK_MONITOR=@0@'.format(conf_data.get('profiling')),
]
message('Maximum number of JACK clients: @0@'.format(conf_data.get('clients')))
message('Maximum number of ports per JACK client: @0@'.format(conf_data.get('ports-per-client')))
message('Build with engine profiling: @0@'.format(conf_data.get('profiling')))

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

# dependencies for jackd
deps_jackdbus = [dep_threads]

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

# dependencies for jack/jack_coreaudio
deps_jack_coreaudio = []

# dependencies for jack/jack_coremidi
deps_jack_coremidi = []

# dependencies for jack/jack_portaudio
deps_jack_portaudio = []

# dependencies for jack/jack_winmme
deps_jack_winmme = []

# dependencies for jack_freewheel
deps_jack_freewheel = []

# dependencies for jack_connect
deps_jack_connect = []

# dependencies for jack_lsp
deps_jack_lsp = []

# dependencies for jack_metro
deps_jack_metro = []

# dependencies for jack_midiseq
deps_jack_midiseq = []

# dependencies for jack_midisine
deps_jack_midisine = []

# dependencies for jack_showtime
deps_jack_showtime = []

# dependencies for jack_simple_client
deps_jack_simple_client = []

# dependencies for jack_zombie
deps_jack_zombie = []

# dependencies for jack_load
deps_jack_load = []

# dependencies for jack_unload
deps_jack_unload = []

# dependencies for jack_alias
deps_jack_alias = []

# dependencies for jack_bufsize
deps_jack_bufsize = []

# dependencies for jack_wait
deps_jack_wait = []

# dependencies for jack_samplerate
deps_jack_samplerate = []

# dependencies for jack_evmon
deps_jack_evmon = []

# dependencies for jack_monitor_client
deps_jack_monitor_client = []

# dependencies for jack_thru
deps_jack_thru = []

# dependencies for jack_cpu_load
deps_jack_cpu_load = []

# dependencies for jack_simple_session_client
deps_jack_simple_session_client = []

# dependencies for jack_session_notify
deps_jack_session_notify = []

# dependencies for jack_server_control
deps_jack_server_control = [lib_stdcpp]

# dependencies for jack_net_slave
deps_jack_net_slave = []

# dependencies for jack_net_master
deps_jack_net_master = []

# dependencies for jack_latent_client
deps_jack_latent_client = []

# dependencies for jack_midi_dump
deps_jack_midi_dump = [dep_threads]

# dependencies for jack_midi_latency_test
deps_jack_midi_latency_test = [dep_threads]

# dependencies for jack_simdtests
deps_jack_simdtests = []

# dependencies for jack_property
deps_jack_property = []

# dependencies for jack_transport
deps_jack_transport = [dep_readline]

# dependencies for jack_rec
deps_jack_rec = [dep_sndfile, dep_threads]

# dependencies for jack_netsource
deps_jack_netsource = [dep_celt, dep_opus, dep_samplerate, lib_m]

# dependencies for alsa_in
deps_alsa_in = [dep_samplerate, lib_m]

# dependencies for alsa_out
deps_alsa_out = [dep_samplerate, lib_m]

# dependencies for inprocess
deps_inprocess = []

# dependencies for jack_test
deps_jack_test = []

# dependencies for jack_cpu
deps_jack_cpu = []

# dependencies for jack_iodelay
deps_jack_iodelay = []

# dependencies for jack_multiple_metro
deps_jack_multiple_metro = []

if dep_celt.found()
  message('Compiling libjack, libjackserver, libjacknet, jack/jack_netone and jack_netsource with legacy celt support.')
endif

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
  cpp_args_jack_client = ['-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"']
  src_jack_client += src_common + src_common_linux + src_jack_client_posix
  include_jack_client = include_directories('.', 'common/jack', 'common', 'linux', 'linux/alsa', 'posix')
  deps_jack_client += [lib_rt, lib_dl]
  link_flags_jack_client = []
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_linux + src_jack_server_posix
  cpp_args_jack_server = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
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
  cpp_args_netmanager = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
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
  link_flags_jackd = []
  # jackdbus
  if dep_dbus.found() and dep_expat.found()
    message('Building jackdbus.')
    src_jackdbus_linux = [
      'linux/uptime.c',
      'linux/JackLinuxTime.c'
    ]
    src_jackdbus += src_jackdbus_posix + src_jackdbus_linux
    include_jackdbus = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
    deps_jackdbus += [dep_expat, dep_dbus, lib_dl, lib_rt, lib_stdcpp]
    link_flags_jackdbus = []
  endif
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
  # jack_freewheel
  include_jack_freewheel = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_freewheel += [lib_m, lib_rt]
  link_flags_jack_freewheel = []
  # jack_connect
  include_jack_connect = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_connect += [lib_m, lib_rt]
  link_flags_jack_connect = []
  # jack_lsp
  include_jack_lsp = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_lsp += [lib_m, lib_rt]
  link_flags_jack_lsp = []
  # jack_metro
  include_jack_metro = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_metro += [lib_m, lib_rt]
  link_flags_jack_metro = []
  # jack_midiseq
  include_jack_midiseq = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_midiseq += [lib_m, lib_rt]
  link_flags_jack_midiseq = []
  # jack_midisine
  include_jack_midisine = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_midisine += [lib_m, lib_rt]
  link_flags_jack_midisine = []
  # jack_showtime
  include_jack_showtime = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_showtime += [lib_m, lib_rt]
  link_flags_jack_showtime = []
  # jack_simple_client
  include_jack_simple_client = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_simple_client += [lib_m, lib_rt]
  link_flags_jack_simple_client = []
  # jack_zombie
  include_jack_zombie = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_zombie += [lib_m, lib_rt]
  link_flags_jack_zombie = []
  # jack_load
  include_jack_load = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_load += [lib_m, lib_rt]
  link_flags_jack_load = []
  # jack_unload
  include_jack_unload = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_unload += [lib_m, lib_rt]
  link_flags_jack_unload = []
  # jack_alias
  include_jack_alias = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_alias += [lib_m, lib_rt]
  link_flags_jack_alias = []
  # jack_bufsize
  include_jack_bufsize = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_bufsize += [lib_m, lib_rt]
  link_flags_jack_bufsize = []
  # jack_wait
  include_jack_wait = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_wait += [lib_m, lib_rt]
  link_flags_jack_wait = []
  # jack_samplerate
  include_jack_samplerate = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_samplerate += [lib_m, lib_rt]
  link_flags_jack_samplerate = []
  # jack_evmon
  include_jack_evmon = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_evmon += [lib_m, lib_rt]
  link_flags_jack_evmon = []
  # jack_monitor_client
  include_jack_monitor_client = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_monitor_client += [lib_m, lib_rt]
  link_flags_jack_monitor_client = []
  # jack_thru
  include_jack_thru = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_thru += [lib_m, lib_rt]
  link_flags_jack_thru = []
  # jack_cpu_load
  include_jack_cpu_load = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_cpu_load += [lib_m, lib_rt]
  link_flags_jack_cpu_load = []
  # jack_simple_session_client
  include_jack_simple_session_client = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_simple_session_client += [lib_m, lib_rt]
  link_flags_jack_simple_session_client = []
  # jack_session_notify
  include_jack_session_notify = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_session_notify += [lib_m, lib_rt]
  link_flags_jack_session_notify = []
  # jack_server_control
  include_jack_server_control = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_server_control += [lib_m, lib_rt]
  link_flags_jack_server_control = []
  # jack_net_slave
  include_jack_net_slave = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_net_slave += [lib_m, lib_rt]
  link_flags_jack_net_slave = []
  # jack_net_master
  include_jack_net_master = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_net_master += [lib_m, lib_rt]
  link_flags_jack_net_master = []
  # jack_latent_client
  include_jack_latent_client = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_latent_client += [lib_m, lib_rt]
  link_flags_jack_latent_client = []
  # jack_midi_dump
  include_jack_midi_dump = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_midi_dump += [lib_m, lib_rt]
  link_flags_jack_midi_dump = []
  # jack_midi_latency_test
  include_jack_midi_latency_test = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_midi_latency_test += [lib_m, lib_rt]
  link_flags_jack_midi_latency_test = []
  # jack_simdtests
  include_jack_simdtests = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_simdtests += [lib_m, lib_rt]
  link_flags_jack_simdtests = []
  # jack_property
  include_jack_property = include_directories('.', 'common/jack', 'common', 'linux', 'posix')
  deps_jack_property += [lib_m, lib_rt]
  link_flags_jack_property = []
  # jack_transport
  include_jack_transport = include_directories('.', 'common/jack', 'common')
  deps_jack_transport += [lib_rt]
  link_flags_jack_transport = []
  # jack_rec
  include_jack_rec = include_directories('.', 'common/jack', 'common')
  deps_jack_rec += [lib_rt]
  link_flags_jack_rec = []
  # jack_netsource
  include_jack_netsource = include_directories('.', 'common/jack', 'common', 'posix')
  link_flags_jack_netsource = []
  # alsa_in
  include_alsa_in = include_directories('.', 'common/jack', 'common')
  deps_alsa_in += [dep_alsa]
  link_flags_alsa_in = []
  # alsa_out
  include_alsa_out = include_directories('.', 'common/jack', 'common')
  deps_alsa_out += [dep_alsa]
  link_flags_alsa_out = []
  # jack/inprocess
  include_inprocess = include_directories('.', 'common/jack', 'common')
  link_flags_inprocess = []
  # jack_test
  include_jack_test = include_directories('common/jack', 'common')
  deps_jack_test += [lib_rt]
  # jack_cpu
  deps_jack_cpu += [lib_rt]
  # jack_iodelay
  deps_jack_iodelay += [lib_rt]
  # jack_multiple_metro
  deps_jack_multiple_metro += [lib_rt]
endif

if os in ['freebsd', 'sunos']
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
  cpp_args_jack_client = ['-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"']
  include_jack_client = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_jack_client += [lib_rt]
  link_flags_jack_client = ['-lnsl', '-lsocket']
  symbol_visibility_jack_client = 'default'
  # libjackserver
  src_jack_server += src_common + src_common_sunos + src_jack_server_posix
  cpp_args_jack_server = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
  include_jack_server = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_jack_server += [lib_rt]
  link_flags_jack_server = ['-lnsl', '-lsocket']
  symbol_visibility_jack_server = 'default'
  # libjacknet
  src_jacknet_sunos = ['solaris/JackSolarisTime.c']
  src_jacknet += src_jacknet_posix + src_jacknet_sunos
  include_jacknet = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_jacknet += [lib_rt]
  link_flags_jacknet = []
  symbol_visibility_jacknet = 'hidden'
  # netmanager
  src_netmanager_sunos = ['solaris/JackSolarisTime.c']
  src_netmanager += src_netmanager_posix + src_netmanager_sunos
  cpp_args_netmanager = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
  include_netmanager = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  deps_netmanager = [dep_threads, lib_dl, lib_rt]
  link_flags_netmanager = []
  symbol_visibility_netmanager = 'hidden'
  # jack/profiler
  src_profiler_sunos = ['solaris/JackSolarisTime.c']
  src_profiler += src_profiler_posix + src_profiler_sunos
  include_profiler = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
  link_flags_profiler = []
  symbol_visibility_profiler = 'hidden'
  # jack/netadapter
  src_netadapter_sunos = ['solaris/JackSolarisTime.c']
  src_netadapter += src_netadapter_posix + src_netadapter_sunos
  include_netadapter = include_directories('.', 'common/jack', 'common', 'posix', 'solaris')
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
  if dep_oss.found()
    message('Building boomer and OSS drivers.')
    # jack/jack_boomer
    src_jack_boomer_sunos = ['solaris/JackSolarisTime.c']
    src_jack_boomer += src_jack_boomer_posix + src_jack_boomer_sunos
    include_jack_boomer = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
    deps_jack_boomer += [lib_dl]
    link_flags_jack_boomer = []
    symbol_visibility_jack_boomer = 'default'
    # jack/jack_oss
    src_jack_oss_sunos = ['solaris/JackSolarisTime.c']
    src_jack_oss += src_jack_oss_posix + src_jack_oss_sunos
    include_jack_oss = include_directories('.', 'common/jack', 'common', 'posix', 'solaris', 'solaris/oss')
    deps_jack_oss += [lib_dl]
    link_flags_jack_oss = []
    symbol_visibility_jack_oss = 'default'
  endif
  # jack_freewheel
  include_jack_freewheel = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_freewheel += [lib_m]
  link_flags_jack_freewheel = []
  # jack_connect
  include_jack_connect = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_connect += [lib_m]
  link_flags_jack_connect = []
  # jack_lsp
  include_jack_lsp = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_lsp += [lib_m]
  link_flags_jack_lsp = []
  # jack_metro
  include_jack_metro = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_metro += [lib_m]
  link_flags_jack_metro = []
  # jack_midiseq
  include_jack_midiseq = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_midiseq += [lib_m]
  link_flags_jack_midiseq = []
  # jack_midisine
  include_jack_midisine = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_midisine += [lib_m]
  link_flags_jack_midisine = []
  # jack_showtime
  include_jack_showtime = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_showtime += [lib_m]
  link_flags_jack_showtime = []
  # jack_simple_client
  include_jack_simple_client = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_simple_client += [lib_m]
  link_flags_jack_simple_client = []
  # jack_zombie
  include_jack_zombie = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_zombie += [lib_m]
  link_flags_jack_zombie = []
  # jack_load
  include_jack_load = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_load += [lib_m]
  link_flags_jack_load = []
  # jack_unload
  include_jack_unload = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_unload += [lib_m]
  link_flags_jack_unload = []
  # jack_alias
  include_jack_alias = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_alias += [lib_m]
  link_flags_jack_alias = []
  # jack_bufsize
  include_jack_bufsize = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_bufsize += [lib_m]
  link_flags_jack_bufsize = []
  # jack_wait
  include_jack_wait = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_wait += [lib_m]
  link_flags_jack_wait = []
  # jack_samplerate
  include_jack_samplerate = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_samplerate += [lib_m]
  link_flags_jack_samplerate = []
  # jack_evmon
  include_jack_evmon = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_evmon += [lib_m]
  link_flags_jack_evmon = []
  # jack_monitor_client
  include_jack_monitor_client = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_monitor_client += [lib_m]
  link_flags_jack_monitor_client = []
  # jack_thru
  include_jack_thru = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_thru += [lib_m]
  link_flags_jack_thru = []
  # jack_cpu_load
  include_jack_cpu_load = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_cpu_load += [lib_m]
  link_flags_jack_cpu_load = []
  # jack_simple_session_client
  include_jack_simple_session_client = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_simple_session_client += [lib_m]
  link_flags_jack_simple_session_client = []
  # jack_session_notify
  include_jack_session_notify = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_session_notify += [lib_m]
  link_flags_jack_session_notify = []
  # jack_server_control
  include_jack_server_control = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_server_control += [lib_m]
  link_flags_jack_server_control = []
  # jack_net_slave
  include_jack_net_slave = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_net_slave += [lib_m]
  link_flags_jack_net_slave = []
  # jack_net_master
  include_jack_net_master = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_net_master += [lib_m]
  link_flags_jack_net_master = []
  # jack_latent_client
  include_jack_latent_client = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_latent_client += [lib_m]
  link_flags_jack_latent_client = []
  # jack_midi_dump
  include_jack_midi_dump = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_midi_dump += [lib_m]
  link_flags_jack_midi_dump = []
  # jack_midi_latency_test
  include_jack_midi_latency_test = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_midi_latency_test += [lib_m]
  link_flags_jack_midi_latency_test = []
  # jack_simdtests
  include_jack_simdtests = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_simdtests += [lib_m]
  link_flags_jack_simdtests = []
  # jack_property
  include_jack_property = include_directories('.', 'common/jack', 'common', 'solaris', 'posix')
  deps_jack_property += [lib_m]
  link_flags_jack_property = []
  # jack_transport
  include_jack_transport = include_directories('.', 'common/jack', 'common')
  deps_jack_transport += []
  link_flags_jack_transport = []
  # jack_rec
  include_jack_rec = include_directories('.', 'common/jack', 'common')
  deps_jack_rec += [lib_rt]
  link_flags_jack_rec = []
  # jack_netsource
  include_jack_netsource = include_directories('.', 'common/jack', 'common', 'posix')
  link_flags_jack_netsource = []
  # jack/inprocess
  include_inprocess = include_directories('.', 'common/jack', 'common')
  deps_inprocess += [lib_m]
  link_flags_inprocess = []
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
  cpp_args_jack_client = ['-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"']
  include_jack_client = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jack_client += [dep_accelerate, dep_coreaudio]
  link_flags_jack_client = ['-single_module']
  symbol_visibility_jack_client = 'hidden'
  # libjackserver
  src_jack_server += src_common + src_common_darwin + src_jack_server_posix
  cpp_args_jack_server = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
  include_jack_server = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jack_server += [dep_accelerate, dep_coreaudio, dep_corefoundation]
  link_flags_jack_server = ['-single_module']
  symbol_visibility_jack_server = 'hidden'
  # libjacknet
  src_jacknet_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jacknet += src_jacknet_posix + src_jacknet_darwin
  include_jacknet = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_jacknet += [dep_coreaudio]
  link_flags_jacknet = ['-single_module']
  symbol_visibility_jacknet = 'hidden'
  # jack/netmanager
  src_netmanager_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_netmanager += src_netmanager_posix + src_netmanager_darwin
  cpp_args_netmanager = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('libdir'), '/jack') + '"',
  ]
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
  src_netadapter_darwin = [
    'macosx/JackMachThread.mm',
    'macosx/JackMachTime.c'
  ]
  src_netadapter += src_netadapter_posix + src_netadapter_darwin
  include_netadapter = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_netadapter += [dep_coreaudio]
  link_flags_netadapter = []
  symbol_visibility_netadapter = 'hidden'
  # jack/audioadapter
  src_audioadapter_darwin = [
    'macosx/coreaudio/JackCoreAudioAdapter.mm',
    'macosx/JackMachThread.mm',
    'macosx/JackMachTime.c'
  ]
  src_audioadapter += src_audioadapter_posix + src_audioadapter_darwin
  include_audioadapter = include_directories('.', 'common/jack', 'common', 'macosx', 'macosx/coreaudio', 'posix')
  deps_audioadapter += [dep_coreaudio, dep_audiounit, dep_audiotoolbox, dep_coreservices]
  link_flags_audioadapter = []
  symbol_visibility_audioadapter = 'hidden'
  # jackd
  src_jackd_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jackd += src_jackd_posix + src_jackd_darwin
  include_jackd = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jackd += [dep_coreaudio, dep_coreservices, lib_dl]
  link_flags_jackd = []
  # jackdbus
  if dep_dbus.found() and dep_expat.found()
    message('Building jackdbus.')
    src_jackdbus_darwin = [
      'macosx/uptime.c',
      'macosx/JackMachThread.mm',
      'macosx/JackMachTime.c'
    ]
    src_jackdbus += src_jackdbus_posix + src_jackdbus_darwin
    include_jackdbus = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
    deps_jackdbus += [dep_coreaudio, dep_expat, dep_dbus, lib_dl]
    link_flags_jackdbus = []
  endif
  # jack/jack_dummy
  src_jack_dummy_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_dummy += src_jack_dummy_posix + src_jack_dummy_darwin
  include_jack_dummy = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_dummy += [dep_coreaudio]
  link_flags_jack_dummy = []
  symbol_visibility_jack_dummy = 'default'
  # jack/jack_loopback
  src_jack_loopback_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_loopback += src_jack_loopback_posix + src_jack_loopback_darwin
  include_jack_loopback = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_loopback += [dep_coreaudio]
  link_flags_jack_loopback = []
  symbol_visibility_jack_loopback = 'default'
  # jack/jack_net
  src_jack_net_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_net += src_jack_net_posix + src_jack_net_darwin
  include_jack_net = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_net += [dep_coreaudio]
  link_flags_jack_net = []
  symbol_visibility_jack_net = 'default'
  # jack/jack_netone
  src_jack_netone_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_netone += src_jack_netone_posix + src_jack_netone_darwin
  include_jack_netone = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_netone += [dep_coreaudio]
  link_flags_jack_netone = []
  symbol_visibility_jack_netone = 'default'
  # jack/jack_proxy
  src_jack_proxy_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
  src_jack_proxy += src_jack_proxy_posix + src_jack_proxy_darwin
  include_jack_proxy = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_proxy += [dep_coreaudio, lib_dl]
  link_flags_jack_proxy = []
  symbol_visibility_jack_proxy = 'default'
  if dep_audiounit.found() and dep_coreaudio.found() and dep_coreservices.found()
    message('Building CoreAudio driver.')
    # jack/jack_coreaudio
    src_jack_coreaudio_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
    src_jack_coreaudio += src_jack_coreaudio_darwin
    cpp_args_jack_coreaudio = []
    include_jack_coreaudio = include_directories('.', 'common/jack', 'common', 'macosx/', 'macosx/coreaudio', 'posix')
    deps_jack_coreaudio += [dep_audiounit, dep_coreaudio, dep_coreservices, lib_aften]
    link_flags_jack_coreaudio = []
    symbol_visibility_jack_coreaudio = 'default'
  endif
  if dep_audiounit.found() and dep_coremidi.found() and dep_coreservices.found()
    # jack/jack_coremidi
    message('Building CoreMidi driver.')
    src_jack_coremidi_darwin = ['macosx/JackMachThread.mm', 'macosx/JackMachTime.c']
    src_jack_coremidi += src_jack_coremidi_darwin
    cpp_args_jack_coremidi = []
    include_jack_coremidi = include_directories('.', 'common/jack', 'common', 'macosx/', 'macosx/coremidi', 'posix')
    deps_jack_coremidi += [dep_audiounit, dep_coreaudio, dep_coremidi, dep_coreservices, dep_foundation]
    link_flags_jack_coremidi = []
    symbol_visibility_jack_coremidi = 'default'
  endif
  # jack_freewheel
  include_jack_freewheel = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_freewheel += [dep_foundation]
  link_flags_jack_freewheel = []
  # jack_connect
  include_jack_connect = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_connect += [dep_foundation]
  link_flags_jack_connect = []
  # jack_lsp
  include_jack_lsp = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_lsp += [dep_foundation]
  link_flags_jack_lsp = []
  # jack_metro
  include_jack_metro = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_metro += [dep_foundation]
  link_flags_jack_metro = []
  # jack_midiseq
  include_jack_midiseq = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_midiseq += [dep_foundation]
  link_flags_jack_midiseq = []
  # jack_midisine
  include_jack_midisine = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_midisine += [dep_foundation]
  link_flags_jack_midisine = []
  # jack_showtime
  include_jack_showtime = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_showtime += [dep_foundation]
  link_flags_jack_showtime = []
  # jack_simple_client
  include_jack_simple_client = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_simple_client += [dep_foundation]
  link_flags_jack_simple_client = []
  # jack_zombie
  include_jack_zombie = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_zombie += [dep_foundation]
  link_flags_jack_zombie = []
  # jack_load
  include_jack_load = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_load += [dep_foundation]
  link_flags_jack_load = []
  # jack_unload
  include_jack_unload = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_unload += [dep_foundation]
  link_flags_jack_unload = []
  # jack_alias
  include_jack_alias = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_alias += [dep_foundation]
  link_flags_jack_alias = []
  # jack_bufsize
  include_jack_bufsize = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_bufsize += [dep_foundation]
  link_flags_jack_bufsize = []
  # jack_wait
  include_jack_wait = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_wait += [dep_foundation]
  link_flags_jack_wait = []
  # jack_samplerate
  include_jack_samplerate = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_samplerate += [dep_foundation]
  link_flags_jack_samplerate = []
  # jack_evmon
  include_jack_evmon = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_evmon += [dep_foundation]
  link_flags_jack_evmon = []
  # jack_monitor_client
  include_jack_monitor_client = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_monitor_client += [dep_foundation]
  link_flags_jack_monitor_client = []
  # jack_thru
  include_jack_thru = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_thru += [dep_foundation]
  link_flags_jack_thru = []
  # jack_cpu_load
  include_jack_cpu_load = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_cpu_load += [dep_foundation]
  link_flags_jack_cpu_load = []
  # jack_simple_session_client
  include_jack_simple_session_client = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_simple_session_client += [dep_foundation]
  link_flags_jack_simple_session_client = []
  # jack_session_notify
  include_jack_session_notify = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_session_notify += [dep_foundation]
  link_flags_jack_session_notify = []
  # jack_server_control
  include_jack_server_control = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_server_control += [dep_foundation]
  link_flags_jack_server_control = []
  # jack_net_slave
  include_jack_net_slave = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_net_slave += [dep_foundation]
  link_flags_jack_net_slave = []
  # jack_net_master
  include_jack_net_master = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_net_master += [dep_foundation]
  link_flags_jack_net_master = []
  # jack_latent_client
  include_jack_latent_client = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_latent_client += [dep_foundation]
  link_flags_jack_latent_client = []
  # jack_midi_dump
  include_jack_midi_dump = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_midi_dump += [dep_foundation]
  link_flags_jack_midi_dump = []
  # jack_midi_latency_test
  include_jack_midi_latency_test = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_midi_latency_test += [dep_foundation]
  link_flags_jack_midi_latency_test = []
  # jack_simdtests
  include_jack_simdtests = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_simdtests += [dep_foundation]
  link_flags_jack_simdtests = []
  # jack_property
  include_jack_property = include_directories('.', 'common/jack', 'common', 'macosx', 'posix')
  deps_jack_property += [dep_foundation]
  link_flags_jack_property = []
  # jack_transport
  include_jack_transport = include_directories('.', 'common/jack', 'common')
  deps_jack_transport += []
  link_flags_jack_transport = []
  # jack_rec
  include_jack_rec = include_directories('.', 'common/jack', 'common')
#  deps_jack_rec += [lib_rt]
  link_flags_jack_rec = []
  # jack_netsource
  include_jack_netsource = include_directories('.', 'common/jack', 'common', 'posix')
  link_flags_jack_netsource = []
  # jack/inprocess
  include_inprocess = include_directories('.', 'common/jack', 'common')
#  deps_inprocess += [lib_m]
  link_flags_inprocess = []
  # jack_test
  include_jack_test = include_directories('common/jack', 'common')
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
  cpp_args_jack_client = ['-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('bindir'), '/jack') + '"']
  include_jack_client = include_directories('.', 'common/jack', 'common', 'windows')
  deps_jack_client += [dep_regex, dep_ws2_32, dep_psapi, dep_winmm]
  link_flags_jack_client = []
  symbol_visibility_jack_client = 'default'
  # libjackserver
  src_jack_server += src_common + src_common_windows + src_jack_server_windows
  cpp_args_jack_server = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('bindir'), '/jack') + '"',
  ]
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
  include_jacknet = include_directories('.', 'common/jack', 'common', 'windows')
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
  cpp_args_netmanager = [
    '-DSERVER_SIDE',
    '-DADDON_DIR="' + join_paths(get_option('prefix'), get_option('bindir'), '/jack') + '"',
  ]
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
  if dep_portaudio.found()
    # jack/jack_portaudio
    message('Building PortAudio driver.')
    src_jack_portaudio_windows = [
      'windows/JackNetWinSocket.cpp',
      'windows/JackWinThread.cpp',
      'windows/JackMMCSS.cpp',
      'windows/JackWinTime.c',
    ]
    src_jack_portaudio += src_jack_portaudio_windows
    include_jack_portaudio = include_directories('.', 'common/jack', 'common', 'windows')
    deps_jack_portaudio += [dep_portaudio]
    link_flags_jack_portaudio = []
    symbol_visibility_jack_portaudio = 'default'
  endif
  if dep_winmme.found()
    # jack/jack_winmme
    message('Building WinMME driver.')
    src_jack_winmme_windows = [
      'windows/JackNetWinSocket.cpp',
      'windows/JackWinThread.cpp',
      'windows/JackMMCSS.cpp',
      'windows/JackWinTime.c',
    ]
    src_jack_winmme += src_jack_winmme_windows
    include_jack_winmme = include_directories('.', 'common/jack', 'common', 'windows')
    deps_jack_winmme += [dep_winmme]
    link_flags_jack_winmme = []
    symbol_visibility_jack_winmme = 'default'
  endif
  # jack_freewheel
  include_jack_freewheel = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_freewheel = []
  # jack_connect
  include_jack_connect = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_connect = []
  # jack_lsp
  include_jack_lsp = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_lsp = []
  # jack_metro
  include_jack_metro = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_metro = []
  # jack_midiseq
  include_jack_midiseq = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_midiseq = []
  # jack_midisine
  include_jack_midisine = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_midisine = []
  # jack_showtime
  include_jack_showtime = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_showtime = []
  # jack_simple_client
  include_jack_simple_client = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_simple_client = []
  # jack_zombie
  include_jack_zombie = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_zombie = []
  # jack_load
  include_jack_load = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_load = []
  # jack_unload
  include_jack_unload = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_unload = []
  # jack_alias
  include_jack_alias = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_alias = []
  # jack_bufsize
  include_jack_bufsize = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_bufsize = []
  # jack_wait
  include_jack_wait = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_wait = []
  # jack_samplerate
  include_jack_samplerate = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_samplerate = []
  # jack_evmon
  include_jack_evmon = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_evmon = []
  # jack_monitor_client
  include_jack_monitor_client = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_monitor_client = []
  # jack_thru
  include_jack_thru = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_thru = []
  # jack_cpu_load
  include_jack_cpu_load = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_cpu_load = []
  # jack_simple_session_client
  include_jack_simple_session_client = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_simple_session_client = []
  # jack_session_notify
  include_jack_session_notify = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_session_notify = []
  # jack_server_control
  include_jack_server_control = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_server_control = []
  # jack_net_slave
  include_jack_net_slave = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_net_slave = []
  # jack_net_master
  include_jack_net_master = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_net_master = []
  # jack_latent_client
  include_jack_latent_client = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_latent_client = []
  # jack_midi_dump
  include_jack_midi_dump = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_midi_dump = []
  # jack_midi_latency_test
  include_jack_midi_latency_test = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_midi_latency_test = []
  # jack_simdtests
  include_jack_simdtests = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_simdtests = []
  # jack_property
  include_jack_property = include_directories('.', 'common/jack', 'common', 'windows')
  link_flags_jack_property = []
  # jack_transport
  include_jack_transport = include_directories('.', 'common/jack', 'common')
  link_flags_jack_transport = []
  # jack_rec
  include_jack_rec = include_directories('.', 'common/jack', 'common')
  link_flags_jack_rec = []
  # jack_netsource
  include_jack_netsource = include_directories('.', 'common/jack', 'common', 'posix')
  link_flags_jack_netsource = []
  # jack/inprocess
  include_inprocess = include_directories('.', 'common/jack', 'common')
  link_flags_inprocess = []
endif

if os in ['darwin', 'linux', 'sunos']
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
  if dep_dbus.found()
    message('Compiling jackd with D-Bus support.')
    src_jackd_linux_dbus = [
      'dbus/audio_reserve.c',
      'dbus/reserve.c',
    ]
    src_jackd += src_jackd_linux_dbus
    deps_jackd += [dep_dbus]
  endif
endif

# libraries
lib_jack = library(
  'jack',
  cpp_args: cpp_args_common + cpp_args_jack_client,
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
  cpp_args: cpp_args_common + cpp_args_jack_server,
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + cpp_args_netmanager,
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
  sources: src_audioadapter,
  include_directories: include_audioadapter,
  dependencies: deps_audioadapter,
  link_with: [lib_jackserver],
  link_args: link_flags_audioadapter,
  gnu_symbol_visibility: symbol_visibility_audioadapter,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)
lib_inprocess = library(
  'inprocess',
  name_prefix: '',
  sources: src_inprocess,
  include_directories: include_inprocess,
  dependencies: deps_inprocess,
  link_with: [lib_jackserver],
  link_args: link_flags_inprocess,
  install: true,
  install_dir: join_paths(get_option('libdir'), 'jack')
)

# drivers
lib_jack_dummy = library(
  'jack_dummy',
  name_prefix: '',
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
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
    cpp_args: cpp_args_common + cpp_args_jack_alsa,
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
    cpp_args: cpp_args_common + cpp_args_jack_alsarawmidi,
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
    cpp_args: cpp_args_common + cpp_args_jack_firewire,
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
    cpp_args: cpp_args_common + cpp_args_jack_iio,
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
if os == 'darwin' and dep_audiounit.found() and dep_coreaudio.found() and dep_coreservices.found() and lib_aften.found()
  lib_jack_coreaudio = library(
    'jack_coreaudio',
    name_prefix: '',
    cpp_args: cpp_args_common + cpp_args_jack_coreaudio,
    sources: src_jack_coreaudio,
    include_directories: include_jack_coreaudio,
    dependencies: deps_jack_coreaudio,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_coreaudio,
    gnu_symbol_visibility: symbol_visibility_jack_coreaudio,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'darwin' and dep_audiounit.found() and dep_coremidi.found() and dep_coreservices.found() and dep_foundation.found()
  lib_jack_coremidi = library(
    'jack_coremidi',
    name_prefix: '',
    cpp_args: cpp_args_common + cpp_args_jack_coremidi,
    sources: src_jack_coremidi,
    include_directories: include_jack_coremidi,
    dependencies: deps_jack_coremidi,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_coremidi,
    gnu_symbol_visibility: symbol_visibility_jack_coremidi,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'sunos' and dep_oss.found()
  lib_jack_oss = library(
    'jack_oss',
    name_prefix: '',
    cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
    sources: src_jack_oss,
    include_directories: include_jack_oss,
    dependencies: deps_jack_oss,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_oss,
    gnu_symbol_visibility: symbol_visibility_jack_oss,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
  lib_jack_boomer = library(
    'jack_boomer',
    name_prefix: '',
    cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
    sources: src_jack_boomer,
    include_directories: include_jack_boomer,
    dependencies: deps_jack_boomer,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_boomer,
    gnu_symbol_visibility: symbol_visibility_jack_boomer,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'windows' and dep_portaudio.found()
  lib_jack_portaudio = library(
    'jack_portaudio',
    name_prefix: '',
    cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
    sources: src_jack_portaudio,
    include_directories: include_jack_portaudio,
    dependencies: deps_jack_portaudio,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_portaudio,
    gnu_symbol_visibility: symbol_visibility_jack_portaudio,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'jack')
  )
endif
if os == 'windows' and dep_winmme.found()
  lib_jack_winmme = library(
    'jack_winmme',
    name_prefix: '',
    cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
    sources: src_jack_winmme,
    include_directories: include_jack_winmme,
    dependencies: deps_jack_winmme,
    link_with: [lib_jackserver],
    link_args: link_flags_jack_winmme,
    gnu_symbol_visibility: symbol_visibility_jack_winmme,
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
  cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
  sources: src_jackd,
  include_directories: include_jackd,
  dependencies: deps_jackd,
  link_with: [lib_jackserver],
  link_args: link_flags_jackd,
  install: true
)

if os in ['darwin', 'linux', 'sunos'] and dep_dbus.found() and dep_expat.found()
  exe_jackdbus = executable(
    'jackdbus',
    cpp_args: cpp_args_common + ['-DSERVER_SIDE'],
    sources: src_jackdbus,
    include_directories: include_jackdbus,
    dependencies: deps_jackdbus,
    link_with: [lib_jackserver],
    link_args: link_flags_jackdbus,
    install: true
  )
endif
exe_jack_freewheel = executable(
  'jack_freewheel',
  cpp_args: cpp_args_common,
  sources: src_jack_freewheel,
  include_directories: include_jack_freewheel,
  dependencies: deps_jack_freewheel,
  link_with: [lib_jack],
  link_args: link_flags_jack_freewheel,
  install: true
)
exe_jack_connect = executable(
  'jack_connect',
  cpp_args: cpp_args_common,
  sources: src_jack_connect,
  include_directories: include_jack_connect,
  dependencies: deps_jack_connect,
  link_with: [lib_jack],
  link_args: link_flags_jack_connect,
  install: true
)
exe_jack_lsp = executable(
  'jack_lsp',
  cpp_args: cpp_args_common,
  sources: src_jack_lsp,
  include_directories: include_jack_lsp,
  dependencies: deps_jack_lsp,
  link_with: [lib_jack],
  link_args: link_flags_jack_lsp,
  install: true
)
exe_jack_metro = executable(
  'jack_metro',
  cpp_args: cpp_args_common,
  sources: src_jack_metro,
  include_directories: include_jack_metro,
  dependencies: deps_jack_metro,
  link_with: [lib_jack],
  link_args: link_flags_jack_metro,
  install: true
)
exe_jack_midiseq = executable(
  'jack_midiseq',
  cpp_args: cpp_args_common,
  sources: src_jack_midiseq,
  include_directories: include_jack_midiseq,
  dependencies: deps_jack_midiseq,
  link_with: [lib_jack],
  link_args: link_flags_jack_midiseq,
  install: true
)
exe_jack_midisine = executable(
  'jack_midisine',
  cpp_args: cpp_args_common,
  sources: src_jack_midisine,
  include_directories: include_jack_midisine,
  dependencies: deps_jack_midisine,
  link_with: [lib_jack],
  link_args: link_flags_jack_midisine,
  install: true
)
exe_jack_showtime = executable(
  'jack_showtime',
  cpp_args: cpp_args_common,
  sources: src_jack_showtime,
  include_directories: include_jack_showtime,
  dependencies: deps_jack_showtime,
  link_with: [lib_jack],
  link_args: link_flags_jack_showtime,
  install: true
)
exe_jack_simple_client = executable(
  'jack_simple_client',
  cpp_args: cpp_args_common,
  sources: src_jack_simple_client,
  include_directories: include_jack_simple_client,
  dependencies: deps_jack_simple_client,
  link_with: [lib_jack],
  link_args: link_flags_jack_simple_client,
  install: true
)
exe_jack_zombie = executable(
  'jack_zombie',
  cpp_args: cpp_args_common,
  sources: src_jack_zombie,
  include_directories: include_jack_zombie,
  dependencies: deps_jack_zombie,
  link_with: [lib_jack],
  link_args: link_flags_jack_zombie,
  install: true
)
exe_jack_load = executable(
  'jack_load',
  cpp_args: cpp_args_common,
  sources: src_jack_load,
  include_directories: include_jack_load,
  dependencies: deps_jack_load,
  link_with: [lib_jack],
  link_args: link_flags_jack_load,
  install: true
)
exe_jack_unload = executable(
  'jack_unload',
  cpp_args: cpp_args_common,
  sources: src_jack_unload,
  include_directories: include_jack_unload,
  dependencies: deps_jack_unload,
  link_with: [lib_jack],
  link_args: link_flags_jack_unload,
  install: true
)
exe_jack_alias = executable(
  'jack_alias',
  cpp_args: cpp_args_common,
  sources: src_jack_alias,
  include_directories: include_jack_alias,
  dependencies: deps_jack_alias,
  link_with: [lib_jack],
  link_args: link_flags_jack_alias,
  install: true
)
exe_jack_bufsize = executable(
  'jack_bufsize',
  cpp_args: cpp_args_common,
  sources: src_jack_bufsize,
  include_directories: include_jack_bufsize,
  dependencies: deps_jack_bufsize,
  link_with: [lib_jack],
  link_args: link_flags_jack_bufsize,
  install: true
)
exe_jack_wait = executable(
  'jack_wait',
  cpp_args: cpp_args_common,
  sources: src_jack_wait,
  include_directories: include_jack_wait,
  dependencies: deps_jack_wait,
  link_with: [lib_jack],
  link_args: link_flags_jack_wait,
  install: true
)
exe_jack_samplerate = executable(
  'jack_samplerate',
  cpp_args: cpp_args_common,
  sources: src_jack_samplerate,
  include_directories: include_jack_samplerate,
  dependencies: deps_jack_samplerate,
  link_with: [lib_jack],
  link_args: link_flags_jack_samplerate,
  install: true
)
exe_jack_evmon = executable(
  'jack_evmon',
  cpp_args: cpp_args_common,
  sources: src_jack_evmon,
  include_directories: include_jack_evmon,
  dependencies: deps_jack_evmon,
  link_with: [lib_jack],
  link_args: link_flags_jack_evmon,
  install: true
)
exe_jack_monitor_client = executable(
  'jack_monitor_client',
  cpp_args: cpp_args_common,
  sources: src_jack_monitor_client,
  include_directories: include_jack_monitor_client,
  dependencies: deps_jack_monitor_client,
  link_with: [lib_jack],
  link_args: link_flags_jack_monitor_client,
  install: true
)
exe_jack_thru = executable(
  'jack_thru',
  cpp_args: cpp_args_common,
  sources: src_jack_thru,
  include_directories: include_jack_thru,
  dependencies: deps_jack_thru,
  link_with: [lib_jack],
  link_args: link_flags_jack_thru,
  install: true
)
exe_jack_cpu_load = executable(
  'jack_cpu_load',
  cpp_args: cpp_args_common,
  sources: src_jack_cpu_load,
  include_directories: include_jack_cpu_load,
  dependencies: deps_jack_cpu_load,
  link_with: [lib_jack],
  link_args: link_flags_jack_cpu_load,
  install: true
)
exe_jack_simple_session_client = executable(
  'jack_simple_session_client',
  cpp_args: cpp_args_common,
  sources: src_jack_simple_session_client,
  include_directories: include_jack_simple_session_client,
  dependencies: deps_jack_simple_session_client,
  link_with: [lib_jack],
  link_args: link_flags_jack_simple_session_client,
  install: true
)
exe_jack_session_notify = executable(
  'jack_session_notify',
  cpp_args: cpp_args_common,
  sources: src_jack_session_notify,
  include_directories: include_jack_session_notify,
  dependencies: deps_jack_session_notify,
  link_with: [lib_jack],
  link_args: link_flags_jack_session_notify,
  install: true
)
exe_jack_server_control = executable(
  'jack_server_control',
  cpp_args: cpp_args_common,
  sources: src_jack_server_control,
  include_directories: include_jack_server_control,
  dependencies: deps_jack_server_control,
  link_with: [lib_jackserver],
  link_args: link_flags_jack_server_control,
  install: true
)
exe_jack_net_slave = executable(
  'jack_net_slave',
  cpp_args: cpp_args_common,
  sources: src_jack_net_slave,
  include_directories: include_jack_net_slave,
  dependencies: deps_jack_net_slave,
  link_with: [lib_jacknet],
  link_args: link_flags_jack_net_slave,
  install: true
)
exe_jack_net_master = executable(
  'jack_net_master',
  cpp_args: cpp_args_common,
  sources: src_jack_net_master,
  include_directories: include_jack_net_master,
  dependencies: deps_jack_net_master,
  link_with: [lib_jacknet],
  link_args: link_flags_jack_net_master,
  install: true
)
exe_jack_latent_client = executable(
  'jack_latent_client',
  cpp_args: cpp_args_common,
  sources: src_jack_latent_client,
  include_directories: include_jack_latent_client,
  dependencies: deps_jack_latent_client,
  link_with: [lib_jack],
  link_args: link_flags_jack_latent_client,
  install: true
)
exe_jack_midi_dump = executable(
  'jack_midi_dump',
  cpp_args: cpp_args_common,
  sources: src_jack_midi_dump,
  include_directories: include_jack_midi_dump,
  dependencies: deps_jack_midi_dump,
  link_with: [lib_jack],
  link_args: link_flags_jack_midi_dump,
  install: true
)
exe_jack_midi_latency_test = executable(
  'jack_midi_latency_test',
  cpp_args: cpp_args_common,
  sources: src_jack_midi_latency_test,
  include_directories: include_jack_midi_latency_test,
  dependencies: deps_jack_midi_latency_test,
  link_with: [lib_jack],
  link_args: link_flags_jack_midi_latency_test,
  install: true
)
exe_jack_simdtests = executable(
  'jack_simdtests',
  cpp_args: cpp_args_common,
  sources: src_jack_simdtests,
  include_directories: include_jack_simdtests,
  dependencies: deps_jack_simdtests,
  link_with: [lib_jack],
  link_args: link_flags_jack_simdtests,
  install: true
)
exe_jack_property = executable(
  'jack_property',
  cpp_args: cpp_args_common,
  sources: src_jack_property,
  include_directories: include_jack_property,
  dependencies: deps_jack_property,
  link_with: [lib_jack],
  link_args: link_flags_jack_property,
  install: true
)
exe_jack_transport = executable(
  'jack_transport',
  cpp_args: cpp_args_common,
  sources: src_jack_transport,
  include_directories: include_jack_transport,
  dependencies: deps_jack_transport,
  link_with: [lib_jack],
  link_args: link_flags_jack_transport,
  install: true
)
exe_jack_rec = executable(
  'jack_rec',
  cpp_args: cpp_args_common,
  sources: src_jack_rec,
  include_directories: include_jack_rec,
  dependencies: deps_jack_rec,
  link_with: [lib_jack],
  link_args: link_flags_jack_rec,
  install: true
)
exe_jack_netsource = executable(
  'jack_netsource',
  c_args: ['-DNO_JACK_ERROR'],
  cpp_args: cpp_args_common,
  sources: src_jack_netsource,
  include_directories: include_jack_netsource,
  dependencies: deps_jack_netsource,
  link_with: [lib_jack],
  link_args: link_flags_jack_netsource,
  install: true
)
if os == 'linux'
  exe_alsa_in = executable(
    'alsa_in',
    c_args: ['-DNO_JACK_ERROR'],
    cpp_args: cpp_args_common,
    sources: src_alsa_in,
    include_directories: include_alsa_in,
    dependencies: deps_alsa_in,
    link_with: [lib_jack],
    link_args: link_flags_alsa_in,
    install: true
  )
  exe_alsa_out = executable(
    'alsa_out',
    c_args: ['-DNO_JACK_ERROR'],
    cpp_args: cpp_args_common,
    sources: src_alsa_out,
    include_directories: include_alsa_out,
    dependencies: deps_alsa_out,
    link_with: [lib_jack],
    link_args: link_flags_alsa_out,
    install: true
  )
endif
if os != 'windows'
  configure_file(
    input: 'example-clients/jack_control',
    output: 'jack_control',
    copy: true,
    install: true,
    install_dir: get_option('bindir')
  )
  meson.add_install_script(
    'tools/meson_create_symlink',
    get_option('bindir'),
    'jack_connect',
    'jack_disconnect'
  )
  exe_jack_test = executable(
    'jack_test',
    cpp_args: cpp_args_common,
    sources: src_jack_test,
    include_directories: ['common'],
    dependencies: deps_jack_test,
    link_with: [lib_jack],
    install: true
  )
  exe_jack_cpu = executable(
    'jack_cpu',
    sources: src_jack_cpu,
    include_directories: ['common'],
    dependencies: deps_jack_cpu,
    link_with: [lib_jack],
    install: true
  )
  exe_jack_iodelay = executable(
    'jack_iodelay',
    cpp_args: cpp_args_common,
    sources: src_jack_iodelay,
    include_directories: ['common'],
    dependencies: deps_jack_iodelay,
    link_with: [lib_jack],
    install: true
  )
  exe_jack_multiple_metro = executable(
    'jack_multiple_metro',
    cpp_args: cpp_args_common,
    sources: src_jack_multiple_metro,
    include_directories: ['common'],
    dependencies: deps_jack_multiple_metro,
    link_with: [lib_jack],
    install: true
  )
endif

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

if os in ['darwin', 'freebsd', 'linux', 'sunos']
  meson.add_postconf_script(
    'tools/meson_create_man_pages',
    conf_data.get('version'),
    os
  )
  meson.add_install_script(
    'tools/meson_install_man_pages',
    get_option('mandir')
  )
endif

if os in ['darwin', 'freebsd', 'linux', 'sunos'] and dep_dbus.found() and dep_expat.found()
  config_dbus = configuration_data(
    {'BINDIR': join_paths(get_option('prefix'), get_option('bindir'))}
  )
  configure_file(
    input: 'dbus/org.jackaudio.service.in',
    output: 'org.jackaudio.service',
    install: true,
    install_dir: dep_dbus.get_pkgconfig_variable('session_bus_services_dir'),
    configuration: config_dbus
  )
endif

# documentation
run_command(
  prog_doxygen,
  env: ['VERSION=' + conf_data.get('version')]
)
if prog_doxygen.found()
  meson.add_install_script(
    'tools/meson_install_documentation',
    get_option('datadir'),
    meson.project_name())
endif
