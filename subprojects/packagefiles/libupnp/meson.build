project('pupnp', 'c',
  version : '1.14.12',
  default_options : [ 'c_std=gnu99', 'warning_level=1' ]
)

libupnp_incdir = include_directories([ '.', 'upnp/inc', 'upnp/src/inc', 'upnp/src/threadutil' ])
libixml_incdir = include_directories([ 'ixml/inc', 'ixml/src/inc' ])

libixml = static_library('ixml',
  'ixml/src/attr.c',
  'ixml/src/document.c',
  'ixml/src/element.c',
  'ixml/src/ixml.c',
  'ixml/src/ixmldebug.c',
  'ixml/src/ixmlmembuf.c',
  'ixml/src/ixmlparser.c',
  'ixml/src/namedNodeMap.c',
  'ixml/src/node.c',
  'ixml/src/nodeList.c',
  include_directories : [ libixml_incdir, libupnp_incdir ],
)

upnp_sources = [
  'upnp/src/api/UpnpActionComplete.c',
  'upnp/src/api/UpnpActionRequest.c',
  'upnp/src/api/UpnpDiscovery.c',
  'upnp/src/api/UpnpEvent.c',
  'upnp/src/api/UpnpEventSubscribe.c',
  'upnp/src/api/UpnpExtraHeaders.c',
  'upnp/src/api/UpnpFileInfo.c',
  'upnp/src/api/UpnpStateVarComplete.c',
  'upnp/src/api/UpnpStateVarRequest.c',
  'upnp/src/api/UpnpSubscriptionRequest.c',
  'upnp/src/api/UpnpString.c',
  'upnp/src/api/upnpapi.c',
  'upnp/src/api/upnpdebug.c',
  'upnp/src/genlib/client_table/GenlibClientSubscription.c',
  'upnp/src/genlib/client_table/client_table.c',
  'upnp/src/genlib/miniserver/miniserver.c',
  'upnp/src/genlib/net/sock.c',
  'upnp/src/genlib/net/http/httpparser.c',
  'upnp/src/genlib/net/http/httpreadwrite.c',
  'upnp/src/genlib/net/http/parsetools.c',
  'upnp/src/genlib/net/http/statcodes.c',
  'upnp/src/genlib/net/http/webserver.c',
  'upnp/src/genlib/net/uri/uri.c',
  'upnp/src/genlib/service_table/service_table.c',
  'upnp/src/genlib/util/list.c',
  'upnp/src/genlib/util/membuffer.c',
  'upnp/src/genlib/util/strintmap.c',
  'upnp/src/genlib/util/upnp_timeout.c',
  'upnp/src/genlib/util/util.c',
  'upnp/src/threadutil/FreeList.c',
  'upnp/src/threadutil/LinkedList.c',
  'upnp/src/threadutil/ThreadPool.c',
  'upnp/src/threadutil/TimerThread.c',
  'upnp/src/urlconfig/urlconfig.c',
]

if get_option('gena') or get_option('optssdp')
  upnp_sources += [
    'upnp/src/uuid/md5.c',
    'upnp/src/uuid/sysdep.c',
    'upnp/src/uuid/uuid.c',
  ]
endif

if get_option('gena')
  upnp_sources += [
    'upnp/src/gena/gena_device.c',
    'upnp/src/gena/gena_ctrlpt.c',
    'upnp/src/gena/gena_callback2.c',
  ]
endif

if get_option('soap')
  upnp_sources += [
    'upnp/src/soap/soap_device.c',
    'upnp/src/soap/soap_ctrlpt.c',
    'upnp/src/soap/soap_common.c',
  ]
endif

if get_option('ssdp')
  upnp_sources += [
    'upnp/src/ssdp/SSDPResultData.c',
    'upnp/src/ssdp/SSDPResultDataCallback.c',
    'upnp/src/ssdp/ssdp_device.c',
    'upnp/src/ssdp/ssdp_ctrlpt.c',
    'upnp/src/ssdp/ssdp_server.c',
  ]
endif

if get_option('tools')
  upnp_sources += [
    'upnp/src/api/upnptools.c',
  ]
endif

cc = meson.get_compiler('c')
ver = meson.project_version().split('.')
uconf = configuration_data()
uconf.set('IXML_HAVE_SCRIPTSUPPORT', true)
uconf.set('UPNP_VERSION_MAJOR', ver[0])
uconf.set('UPNP_VERSION_MINOR', ver[1])
uconf.set('UPNP_VERSION_PATCH', ver[2])
uconf.set('UPNP_ENABLE_BLOCKING_TCP_CONNECTIONS', get_option('blocking_tcp_connectionsr'))
uconf.set10('UPNP_HAVE_CLIENT', get_option('client'))
uconf.set10('UPNP_HAVE_DEVICE', get_option('device'))
uconf.set10('UPNP_HAVE_GENA', get_option('gena'))
uconf.set10('UPNP_HAVE_TOOLS', get_option('tools'))
uconf.set('UPNP_ENABLE_IPV6', get_option('ipv6'))
uconf.set('UPNP_HAVE_OPTSSDP', get_option('optssdp'))
uconf.set('UPNP_ENABLE_OPEN_SSL', get_option('open_ssl'))
uconf.set10('UPNP_HAVE_SOAP', get_option('soap'))
uconf.set10('UPNP_HAVE_SSDP', get_option('ssdp'))
uconf.set('UPNP_ENABLE_UNSPECIFIED_SERVER', get_option('unspecified_server'))
uconf.set10('UPNP_HAVE_WEBSERVER', get_option('webserver'))
uconf.set('UPNP_MINISERVER_REUSEADDR', get_option('reuseaddr'))

aconf = configuration_data()
aconf.merge_from(uconf)

uconf.set('UPNP_VERSION_STRING', meson.project_version())
aconf.set_quoted('UPNP_VERSION_STRING', meson.project_version())
aconf.set10('UPNP_HAVE_DEBUG', get_option('debug'))
aconf.set10('UPNP_USE_RWLOCK', cc.has_header_symbol('pthread.h', 'pthread_rwlock_t', prefix : '#define _XOPEN_SOURCE 500'))
aconf.set10('HAVE_STRNDUP', cc.has_function('strndup'))
aconf.set10('HAVE_STRNLEN', cc.has_function('strnlen'))

ufile = configure_file(
  input: 'upnp/inc/upnpconfig.h.cm',
  output : 'upnpconfig.h',
  format: 'cmake',
  configuration : uconf
)

afile = configure_file(
  output: 'autoconfig.h',
  configuration: aconf
)

deps = []
if host_machine.system() == 'windows'
  deps += cc.find_library('ws2_32')
  deps += cc.find_library('iphlpapi')
elif host_machine.system() == 'sunos'
  deps += cpp.find_library('nsl')
  deps += cpp.find_library('socket')
endif
deps += dependency('libcrypto', required : get_option('open_ssl'))
deps += dependency('libssl', required : get_option('open_ssl'))
threads_dep = dependency('threads')

libupnp = library('upnp',
  upnp_sources,
  include_directories : [ libixml_incdir, libupnp_incdir ],
  dependencies : [ deps, threads_dep ],
  link_with : libixml,
  install : true,
)

pkg = import('pkgconfig')
pkg.generate(libupnp, name: 'libupnp', subdirs: 'upnp')

headers = [
  'ixml/inc/ixml.h',
  'ixml/inc/ixmldebug.h',
  'upnp/inc/Callback.h',
  'upnp/inc/list.h',
  'upnp/inc/upnp.h',
  'upnp/inc/UpnpActionComplete.h',
  'upnp/inc/UpnpActionRequest.h',
  'upnp/inc/UpnpDiscovery.h',
  'upnp/inc/UpnpEvent.h',
  'upnp/inc/UpnpEventSubscribe.h',
  'upnp/inc/UpnpExtraHeaders.h',
  'upnp/inc/UpnpFileInfo.h',
  'upnp/inc/UpnpGlobal.h',
  'upnp/inc/UpnpInet.h',
  'upnp/inc/UpnpIntTypes.h',
  'upnp/inc/UpnpStateVarComplete.h',
  'upnp/inc/UpnpStateVarRequest.h',
  'upnp/inc/UpnpStdInt.h',
  'upnp/inc/UpnpString.h',
  'upnp/inc/UpnpSubscriptionRequest.h',
  'upnp/inc/UpnpUniStd.h',
  ufile,
]

if get_option('tools')
  headers += 'upnp/inc/upnptools.h'
endif

install_headers(headers, subdir : 'upnp')

libupnp_dep = declare_dependency(
  include_directories : [ libixml_incdir, libupnp_incdir ],
  link_with : libupnp,
)

if get_option('samples') and get_option('tools')
  sample_conf = configuration_data()
  sample_file = configure_file(
    output : 'config_sample.h',
    configuration : sample_conf,
  )

  if get_option('client')
    executable('tv_ctrlpt',
      'upnp/sample/common/sample_util.c',
      'upnp/sample/common/tv_ctrlpt.c',
      'upnp/sample/linux/tv_ctrlpt_main.c',
      include_directories : [ '.', 'ixml/inc','upnp/inc', 'upnp/sample/common' ],
      dependencies : threads_dep,
      link_with : libupnp,
      install : true,
    )
  endif

  if get_option('client') and get_option('device')
    executable('tv_combo',
      'upnp/sample/common/sample_util.c',
      'upnp/sample/common/tv_ctrlpt.c',
      'upnp/sample/common/tv_device.c',
      'upnp/sample/linux/tv_combo_main.c',
      include_directories : [ '.', 'ixml/inc','upnp/inc', 'upnp/sample/common' ],
      dependencies : threads_dep,
      link_with : libupnp,
      install : true,
    )
  endif

  if get_option('device')
    executable('tv_device',
      'upnp/sample/common/sample_util.c',
      'upnp/sample/common/tv_device.c',
      'upnp/sample/linux/tv_device_main.c',
      include_directories : [ '.', 'ixml/inc','upnp/inc', 'upnp/sample/common' ],
      dependencies : threads_dep,
      link_with : libupnp,
      install : true,
    )

    install_subdir(
      'upnp/sample/web',
      install_dir : 'share/upnp',
      strip_directory : true,
    )
  endif
endif
