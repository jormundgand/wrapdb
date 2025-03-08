project('libnpupnp', 'cpp',
  version : '4.2.2',
  default_options : [ 'cpp_std=c++14' ],
  meson_version : '>=0.48',
)

cpp = meson.get_compiler('cpp')

deps = []
if host_machine.system() == 'windows'
    deps += cpp.find_library('ws2_32')
    deps += cpp.find_library('iphlpapi')
elif host_machine.system() == 'sunos'
    deps += cpp.find_library('nsl')
    deps += cpp.find_library('socket')
endif

deps += dependency('threads')
deps += dependency('libmicrohttpd')
deps += dependency('libcurl')
expat_dep = dependency('expat', required : get_option('expat'))
deps += expat_dep

if (host_machine.system() == 'windows')
  add_project_arguments('-DDLL_EXPORT', language : 'cpp')
endif

add_project_arguments('-DSMALLUT_NO_REGEX', language : 'cpp')
add_project_arguments('-DSMALLUT_EXTERNAL_INSTANTIATIONS', language : 'cpp')

libnpupnp_incdir = include_directories([ '.', 'inc', 'src/inc' ])

npupnp_sources = [
  'src/api/upnpdebug.cpp',
  'src/api/upnpapi.cpp',
  'src/dispatcher/miniserver.cpp',
  'src/threadutil/ThreadPool.cpp',
  'src/threadutil/TimerThread.cpp',
  'src/utils/description.cpp',
  'src/utils/genut.cpp',
  'src/utils/httputils.cpp',
  'src/utils/inet_pton.cpp',
  'src/utils/md5.cpp',
  'src/utils/netif.cpp',
  'src/utils/smallut.cpp',
  'src/utils/statcodes.cpp',
  'src/utils/uri.cpp',
  'src/utils/utf8iter.cpp',
  'src/webserver/webserver.cpp',
]

if get_option('gena')
  npupnp_sources += [
    'src/gena/gena_callback2.cpp',
    'src/gena/gena_ctrlpt.cpp',
    'src/gena/gena_device.cpp',
    'src/gena/gena_sids.cpp',
    'src/gena/service_table.cpp',
  ]
endif

if get_option('soap')
  npupnp_sources += [
    'src/soap/soap_ctrlpt.cpp',
    'src/soap/soap_device.cpp',
  ]
endif

if get_option('ssdp')
  npupnp_sources += [
    'src/ssdp/ssdp_ctrlpt.cpp',
    'src/ssdp/ssdp_device.cpp',
    'src/ssdp/ssdpparser.cpp',
    'src/ssdp/ssdp_server.cpp',
  ]
endif

if get_option('tools')
  npupnp_sources += [
    'src/api/upnptools.cpp',
  ]
endif

libnpupnp = library('npupnp',
  npupnp_sources,
  include_directories : libnpupnp_incdir,
  gnu_symbol_visibility : 'hidden',
  version : '4.2.2',
  dependencies : deps,
  install : true,
)

auto = configuration_data()
auto.set_quoted('PACKAGE_VERSION', meson.project_version())
auto.set_quoted('NPUPNP_VERSION_STRING', meson.project_version())
ver = meson.project_version().split('.')
auto.set('NPUPNP_VERSION_MAJOR', ver[0])
auto.set('NPUPNP_VERSION_MINOR', ver[1])
auto.set('NPUPNP_VERSION_PATCH', ver[2])
auto.set('LIBUPNP_EXPORTS', 1)
auto.set10('UPNP_HAVE_DEBUG', get_option('debug'))
auto.set10('UPNP_HAVE_DEVICE', get_option('device'))
auto.set('USE_EXPAT', expat_dep.found())
auto.set10('UPNP_HAVE_GENA', get_option('gena'))
auto.set('UPNP_HAVE_OPTSSDP', get_option('optssdp'))
auto.set10('UPNP_HAVE_SOAP', get_option('soap'))
auto.set10('UPNP_HAVE_SSDP', get_option('ssdp'))
auto.set10('UPNP_HAVE_TOOLS', get_option('tools') or get_option('webserver'))
auto.set('UPNP_ENABLE_UNSPECIFIED_SERVER', get_option('unspecified_server'))
auto.set('UPNP_ENABLE_IPV6', get_option('ipv6'))
auto.set10('UPNP_HAVE_CLIENT', get_option('client'))
auto.set10('UPNP_HAVE_WEBSERVER', get_option('webserver'))
auto.set('HAVE_INET_PTON', true) #every platform has this

ufile = configure_file(
  output : 'upnpconfig.h',
  configuration : auto
)

cfile = configure_file(
  output : 'autoconfig.h',
  configuration : auto
)

pkg = import('pkgconfig')
pkg.generate(libnpupnp, name: 'libnpupnp', subdirs: 'npupnp')

install_headers(
  'inc/upnpdebug.h',
  'inc/UpnpGlobal.h',
  'inc/upnp.h',
  'inc/UpnpInet.h',
  'inc/upnptools.h',
  'src/inc/netif.h',
  ufile,
  subdir: 'npupnp'
)

libnpupnp_dep = declare_dependency(
  include_directories : libnpupnp_incdir,
  link_with : libnpupnp,
)
