project(
  'dnsdist',
  ['c', 'cpp'],
  version: run_command('../../builder-support' / 'gen-version', check: true).stdout().strip(),
  license: 'GPLv2',
  license_files: 'NOTICE',
  meson_version: '>= 1.2.1',
  default_options: [
    'buildtype=debugoptimized',
    'warning_level=2',          # TODO Move this to 3 to enable -Wpedantic
    'cpp_std=c++17',
    'b_pie=true',
  ],
)
add_project_arguments('-DDNSDIST', language: 'cpp')


product_source_dir = meson.current_source_dir()
product_build_dir = meson.current_build_dir()
summary('Source Dir', product_source_dir, section: 'Build')
summary('Build Dir', product_build_dir, section: 'Build')

# Create the configuration object and dependencies list.
conf = configuration_data()
conf.set_quoted('PACKAGE_STRING', 'dnsdist ' + meson.project_version(), description: 'dnsdist version')
conf.set_quoted('PACKAGE_VERSION', meson.project_version(), description: 'version')

# Feature detection and system configuration
subdir('meson' / 'config')                  # Config
subdir('meson' / 'version')                 # Generate version define
subdir('meson' / 'compiler-setup')          # Common compiler setup
subdir('meson' / 'summary')                 # Print a system/project summary
subdir('meson' / 'sysconfdir')              # Sysconfdir
subdir('meson' / 'platform')                # Platform detection
subdir('meson' / 'timet-size')              # Check the size of time_t
subdir('meson' / 'timet-sign')              # Check the sign of time_t
subdir('meson' / 'atomics')                 # Check atomics support
subdir('meson' / 'pthread-headers')         # Check pthread headers
subdir('meson' / 'pthread-setname')         # Pthread setname madness
subdir('meson' / 'pthread-np')              # Pthread _np functions
subdir('meson' / 'strerror')                # Strerror_r
subdir('meson' / 'lua')                     # Lua
subdir('meson' / 'hardening')               # Hardening
subdir('meson' / 'net-libs')                # Network Libraries
subdir('meson' / 'tm-gmtoff')               # Check for tm_gmtoff field in struct tm
subdir('meson' / 'mmap')                    # Check for mmap
subdir('meson' / 'libcap')                  # Libcap to drop capabilities
subdir('meson' / 'libedit')                 # Libedit
subdir('meson' / 'libsodium')               # Libsodium
subdir('meson' / 'libcrypto')               # OpenSSL libcrypto
subdir('meson' / 'libssl')                  # OpenSSL libssl
subdir('meson' / 'libssl-providers')        # OpenSSL libssl providers
subdir('meson' / 'libsnmp')                 # SNMP
subdir('meson' / 'clock-gettime')           # Clock_gettime
subdir('meson' / 'boost')                   # Boost
subdir('meson' / 'boost-test')              # Boost Testing Library
subdir('meson' / 'reproducible')            # Reproducible Builds
subdir('meson' / 'libsystemd')              # Systemd notification
subdir('meson' / 'systemd')                 # Systemd and unit file handling
subdir('meson' / 'code-coverage')           # Code coverage
subdir('meson' / 'auto-var-init')           # Automatic Variable Initialization
subdir('meson' / 'sanitizers')              # Sanitizers
subdir('meson' / 'various-functions')       # Various Functions
subdir('meson' / 'various-headers')         # Various Headers
subdir('meson' / 'dnstap')                  # DNSTAP through libfstream
subdir('meson' / 'dnscrypt')                # DNSCrypt through libsodium
subdir('meson' / 'ipcipher')                # IPCipher (requires libcrypto)
subdir('meson' / 'cdb')                     # CDB
subdir('meson' / 'ebpf')                    # eBPF
subdir('meson' / 'gnutls')                  # GNUTLS
subdir('meson' / 'h2o')                     # H2O
subdir('meson' / 'lmdb')                    # LMDB
subdir('meson' / 'nghttp2')                 # NGHTTP2
subdir('meson' / 'quiche')                  # Quiche
subdir('meson' / 're2')                     # RE2
subdir('meson' / 'xsk')                     # BPF and XDP for XSK
subdir('meson' / 'dot')                     # DNS over TLS
subdir('meson' / 'doh2')                    # DNS over HTTP/2
subdir('meson' / 'doq')                     # DNS over QUIC
subdir('meson' / 'doh3')                    # DNS over HTTP/3
subdir('meson' / 'yaml-configuration')      # YAML configuration

common_sources = []

fs = import('fs')
src_dir = fs.is_dir('.') ? '.' : ''
docs_dir = 'docs'
# Toplevel includes
dep_pdns = declare_dependency(include_directories: include_directories('.', src_dir))

# Ext
subdir('ext' / 'arc4random')
subdir('ext' / 'ipcrypt')
subdir('ext' / 'json11')
subdir('ext' / 'luawrapper')
subdir('ext' / 'protozero')
subdir('ext' / 'yahttp')

if get_option('lmdb').allowed()
  subdir('ext' / 'lmdb-safe')
else
  dep_lmdb_safe = declare_dependency()
endif

if get_option('yaml').allowed()
  subdir('dnsdist-rust-lib')
else
  dep_dnsdist_rust_lib = declare_dependency()
endif

common_sources += files(
  src_dir / 'bpf-filter.cc',
  src_dir / 'capabilities.cc',
  src_dir / 'channel.cc',
  src_dir / 'coverage.cc',
  src_dir / 'credentials.cc',
  src_dir / 'dns.cc',
  src_dir / 'dnscrypt.cc',
  src_dir / 'dnsdist-actions.cc',
  src_dir / 'dnsdist-actions-factory.cc',
  src_dir / 'dnsdist-async.cc',
  src_dir / 'dnsdist-backend.cc',
  src_dir / 'dnsdist-cache.cc',
  src_dir / 'dnsdist-carbon.cc',
  src_dir / 'dnsdist-configuration.cc',
  src_dir / 'dnsdist-configuration-yaml.cc',
  src_dir / 'dnsdist-console.cc',
  src_dir / 'dnsdist-crypto.cc',
  src_dir / 'dnsdist-discovery.cc',
  src_dir / 'dnsdist-dnscrypt.cc',
  src_dir / 'dnsdist-dnsparser.cc',
  src_dir / 'dnsdist-dnsquestion.cc',
  src_dir / 'dnsdist-doh-common.cc',
  src_dir / 'dnsdist-dynblocks.cc',
  src_dir / 'dnsdist-dynbpf.cc',
  src_dir / 'dnsdist-ecs.cc',
  src_dir / 'dnsdist-edns.cc',
  src_dir / 'dnsdist-frontend.cc',
  src_dir / 'dnsdist-healthchecks.cc',
  src_dir / 'dnsdist-idstate.cc',
  src_dir / 'dnsdist-internal-queries.cc',
  src_dir / 'dnsdist-kvs.cc',
  src_dir / 'dnsdist-lbpolicies.cc',
  src_dir / 'dnsdist-lua-actions.cc',
  src_dir / 'dnsdist-lua-bindings.cc',
  src_dir / 'dnsdist-lua-bindings-dnscrypt.cc',
  src_dir / 'dnsdist-lua-bindings-dnsparser.cc',
  src_dir / 'dnsdist-lua-bindings-dnsquestion.cc',
  src_dir / 'dnsdist-lua-bindings-kvs.cc',
  src_dir / 'dnsdist-lua-bindings-network.cc',
  src_dir / 'dnsdist-lua-bindings-packetcache.cc',
  src_dir / 'dnsdist-lua-bindings-protobuf.cc',
  src_dir / 'dnsdist-lua-bindings-rings.cc',
  src_dir / 'dnsdist-lua-configuration-items.cc',
  src_dir / 'dnsdist-lua.cc',
  src_dir / 'dnsdist-lua-ffi.cc',
  src_dir / 'dnsdist-lua-hooks.cc',
  src_dir / 'dnsdist-lua-inspection.cc',
  src_dir / 'dnsdist-lua-inspection-ffi.cc',
  src_dir / 'dnsdist-lua-network.cc',
  src_dir / 'dnsdist-lua-rules.cc',
  src_dir / 'dnsdist-lua-vars.cc',
  src_dir / 'dnsdist-lua-web.cc',
  src_dir / 'dnsdist-mac-address.cc',
  src_dir / 'dnsdist-metrics.cc',
  src_dir / 'dnsdist-nghttp2.cc',
  src_dir / 'dnsdist-nghttp2-in.cc',
  src_dir / 'dnsdist-protobuf.cc',
  src_dir / 'dnsdist-protocols.cc',
  src_dir / 'dnsdist-proxy-protocol.cc',
  src_dir / 'dnsdist-query-count.cc',
  src_dir / 'dnsdist-random.cc',
  src_dir / 'dnsdist-resolver.cc',
  src_dir / 'dnsdist-rings.cc',
  src_dir / 'dnsdist-rule-chains.cc',
  src_dir / 'dnsdist-rules.cc',
  src_dir / 'dnsdist-secpoll.cc',
  src_dir / 'dnsdist-session-cache.cc',
  src_dir / 'dnsdist-self-answers.cc',
  src_dir / 'dnsdist-snmp.cc',
  src_dir / 'dnsdist-svc.cc',
  src_dir / 'dnsdist-systemd.cc',
  src_dir / 'dnsdist-tcp.cc',
  src_dir / 'dnsdist-tcp-downstream.cc',
  src_dir / 'dnsdist-web.cc',
  src_dir / 'dnsdist-xsk.cc',
  src_dir / 'dnsname.cc',
  src_dir / 'dnsparser.cc',
  src_dir / 'dnswriter.cc',
  src_dir / 'dolog.cc',
  src_dir / 'ednscookies.cc',
  src_dir / 'ednsextendederror.cc',
  src_dir / 'ednsoptions.cc',
  src_dir / 'ednssubnet.cc',
  src_dir / 'gettime.cc',
  src_dir / 'ipcipher.cc',
  src_dir / 'iputils.cc',
  src_dir / 'libssl.cc',
  src_dir / 'misc.cc',
  src_dir / 'protozero.cc',
  src_dir / 'proxy-protocol.cc',
  src_dir / 'qtype.cc',
  src_dir / 'remote_logger.cc',
  src_dir / 'snmp-agent.cc',
  src_dir / 'statnode.cc',
  src_dir / 'svc-records.cc',
  src_dir / 'tcpiohandler.cc',
  src_dir / 'threadname.cc',
  src_dir / 'uuid-utils.cc',
  src_dir / 'xsk.cc',
)

conditional_sources = {
  'cdb': {
    'sources': [
      src_dir / 'cdb.cc',
    ],
    'condition': dep_cdb.found(),
  },
  'doh': {
    'sources': [
      src_dir / 'doh.cc',
    ],
    'condition': dep_libh2o_evloop.found(),
   },
  'doh3': {
    'sources': [
      src_dir / 'doh3.cc',
    ],
    'condition': get_option('dns-over-http3'),
   },
  'doq': {
    'sources': [
      src_dir / 'doq.cc',
    ],
    'condition': get_option('dns-over-quic'),
   },
  'ipcipher': {
    'sources': [
      src_dir / 'ipcipher.cc',
    ],
    'condition': dep_libcrypto.found(),
  },
  'quiche': {
    'sources': [
      src_dir / 'doq-common.cc',
    ],
    'condition': dep_libquiche.found(),
   },
   'yaml': {
     'sources': [
       src_dir / 'dnsdist-rust-lib/dnsdist-configuration-yaml-items-generated.cc',
     ],
     'condition': dep_dnsdist_rust_lib.found(),
   }
}

foreach name, info: conditional_sources
  if info['condition']
    common_sources += files(info['sources'])
  endif
endforeach

mplexer_sources = [src_dir / 'pollmplexer.cc']
if have_linux
  mplexer_sources += src_dir / 'epollmplexer.cc'
endif
if have_darwin
  mplexer_sources += src_dir / 'kqueuemplexer.cc'
endif
if have_openbsd
  mplexer_sources += src_dir / 'kqueuemplexer.cc'
endif
if have_freebsd
  mplexer_sources += src_dir / 'kqueuemplexer.cc'
endif
if have_sunos
  mplexer_sources += src_dir / 'devpollmplexer.cc'
  mplexer_sources += src_dir / 'portsmplexer.cc'
endif

# Generate config.h
config_h = configure_file(configuration: conf, output: 'config.h')

sh_program = find_program('sh')
dep_no_config_in_source_check = custom_target(
    input: [],
    output: ['check absense of config.h file in source directory'],
    command: [sh_program, '-c', 'test ! -e @SOURCE_ROOT@/config.h'],
    build_always_stale: true,
)
dep_no_config_in_source = declare_dependency(
  sources: dep_no_config_in_source_check
)

html_sources = [
  src_dir / 'html/index.html',
  src_dir / 'html/local.js',
  src_dir / 'html/js/moment.min.js',
  src_dir / 'html/js/rickshaw.min.js',
  src_dir / 'html/js/d3.min.js',
  src_dir / 'html/js/moment.min.js',
  src_dir / 'html/lines.css',
  src_dir / 'html/legend.css',
  src_dir / 'html/detail.css',
  src_dir / 'html/graph.css',
  src_dir / 'html/powerdns-logo-220px.png',
]

incfiles = find_program('incfiles')

htmlfiles = custom_target(
  command: [incfiles, '@SOURCE_ROOT@'],
  input: html_sources,
  output: 'htmlfiles.h',
  capture: true
)

dep_htmlfiles = declare_dependency(
  sources: [htmlfiles],
)

generate_ffi_interface = find_program('generate-ffi-interface.sh')
ffi_interface = custom_target(
  command: [generate_ffi_interface, '@INPUT@'],
  input: ['dnsdist-lua-ffi-interface.h', 'dnsdist-lua-inspection-ffi.h'],
  output: 'dnsdist-lua-ffi-interface.inc',
  capture: true
)

dep_ffi_interface = declare_dependency(
  sources: [ffi_interface],
)

deps = [
  dep_pdns,
  dep_no_config_in_source,
  dep_arc4random,
  dep_boost,
  dep_cdb,
  dep_dnstap,
  dep_ffi_interface,
  dep_htmlfiles,
  dep_ipcrypt,
  dep_libcap,
  dep_libcrypto,
  dep_gnutls,
  dep_libedit,
  dep_libh2o_evloop,
  dep_json11,
  dep_libnghttp2,
  dep_libquiche,
  dep_libsnmp,
  dep_libsodium,
  dep_libssl,
  dep_lmdb,
  dep_lmdb_safe,
  dep_lua,
  dep_protozero,
  dep_yahttp,
  dep_libre2,
  dep_threads,
  dep_libbpf,
  dep_libxdp,
  dep_dnsdist_rust_lib,
]

libdnsdist_dnslabeltext_source = src_dir / 'dnslabeltext.rl'
libdnsdist_dnslabeltext_gen    = src_dir / 'dnslabeltext.cc'
if not fs.is_file(libdnsdist_dnslabeltext_gen)
  ragel = find_program('ragel', required: true)

  summary('Ragel', ragel.found(), bool_yn: ragel.found(), section: 'DNS Labels')
  summary('Ragel Path', ragel.full_path(), section: 'DNS Labels')
  summary('Ragel Version', ragel.version(), section: 'DNS Labels')

  ragel_generator = generator(
    ragel,
    output: '@BASENAME@.cc',
    arguments: ['@INPUT@', '-o', '@OUTPUT@'],
  )

  libdnsdist_dnslabeltext_gen = ragel_generator.process(libdnsdist_dnslabeltext_source)
endif

libdnsdist_dnslabeltext = declare_dependency(
  link_with: static_library(
    'dnsdist-dnslabeltext',
    libdnsdist_dnslabeltext_gen,
    dependencies: deps,
  )
)

libdnsdist_common = declare_dependency(
  link_with: static_library(
    'dnsdist-common',
    common_sources,
    config_h,
    dependencies: [
      deps,
      libdnsdist_dnslabeltext,
    ],
  )
)

tools = {
  'dnsdist': {
    'main': src_dir / 'dnsdist.cc',
    'files-extra': [
      src_dir / 'dnstap.cc',
      src_dir / 'fstrm_logger.cc',
      mplexer_sources,
    ],
    'manpages': ['dnsdist.1'],
    'deps-extra': [
      dep_pdns,
      dep_boost,
      dep_lua,
      dep_protozero,
      dep_yahttp,
      dep_json11,
      dep_systemd,
    ],
  },
}

if get_option('fuzz-targets')
  fuzz_extra_sources = []
  fuzzer_ldflags = []
  # https://github.com/harfbuzz/harfbuzz/pull/2549/files
  if get_option('fuzzer_ldflags') == ''
    fuzz_extra_sources += src_dir / 'standalone_fuzz_target_runner.cc'
  else
    fuzzer_ldflags += get_option('fuzzer_ldflags')
  endif
  tools += {
    'fuzz-target-dnsdistcache' : {
      'main': src_dir / 'fuzz_dnsdistcache.cc',
      'link-args': fuzzer_ldflags,
      'files-extra': fuzz_extra_sources
    },
  }
  if get_option('xsk').allowed() and dep_libbpf.found() and dep_libxdp.found()
    tools += {
      'fuzz-target-xsk' : {
         'main': src_dir / 'fuzz_xsk.cc',
         'link-args': fuzzer_ldflags,
         'files-extra': fuzz_extra_sources + [
           src_dir / 'dnsname.cc',
           src_dir / 'gettime.cc',
           src_dir / 'iputils.cc',
           src_dir / 'misc.cc',
           src_dir / 'xsk.cc'
           ]
       },
    }
  endif
endif

test_sources = []
test_sources += files(
  src_dir / 'test-base64_cc.cc',
  src_dir / 'test-channel.cc',
  src_dir / 'test-connectionmanagement_hh.cc',
  src_dir / 'test-credentials_cc.cc',
  src_dir / 'test-delaypipe_hh.cc',
  src_dir / 'test-dnscrypt_cc.cc',
  src_dir / 'test-dnsdistasync.cc',
  src_dir / 'test-dnsdistbackend_cc.cc',
  src_dir / 'test-dnsdistbackoff.cc',
  src_dir / 'test-dnsdist_cc.cc',
  src_dir / 'test-dnsdist-connections-cache.cc',
  src_dir / 'test-dnsdist-dnsparser.cc',
  src_dir / 'test-dnsdistdynblocks_hh.cc',
  src_dir / 'test-dnsdistedns.cc',
  src_dir / 'test-dnsdistkvs_cc.cc',
  src_dir / 'test-dnsdistlbpolicies_cc.cc',
  src_dir / 'test-dnsdist-lua-ffi.cc',
  src_dir / 'test-dnsdistluanetwork.cc',
  src_dir / 'test-dnsdistnghttp2_cc.cc',
  src_dir / 'test-dnsdistnghttp2-in_cc.cc',
  src_dir / 'test-dnsdistpacketcache_cc.cc',
  src_dir / 'test-dnsdistrings_cc.cc',
  src_dir / 'test-dnsdistrules_cc.cc',
  src_dir / 'test-dnsdistsvc_cc.cc',
  src_dir / 'test-dnsdisttcp_cc.cc',
  src_dir / 'test-dnsparser_cc.cc',
  src_dir / 'test-iputils_hh.cc',
  src_dir / 'test-luawrapper.cc',
  src_dir / 'test-mplexer.cc',
  src_dir / 'test-proxy_protocol_cc.cc',
  src_dir / 'test-sholder_hh.cc',
)

if get_option('unit-tests')
  libdnsdist_test = declare_dependency(
    link_whole: static_library(
      'dnsdist-test',
      config_h,
      test_sources,
      dependencies: [
          dep_boost,
          dep_boost_test,
          dep_lua,
      ],
    )
  )
  tools += {
    'testrunner': {
        'main': [
          src_dir / 'testrunner.cc',
          mplexer_sources,
        ],
        'deps-extra': [
          libdnsdist_test,
          dep_boost_test,
        ],
    }
  }
endif

man_pages = []
foreach tool, info: tools
  var_name = tool.underscorify()
  main = files(info['main'])

  export_dynamic = 'export-dynamic' in info ? info['export-dynamic'] : false
  files_extra = 'files-extra' in info ? info['files-extra'] : []
  deps_extra = 'deps-extra' in info ? info['deps-extra'] : []
  link_args = 'link-args' in info ? info['link-args'] : []

  set_variable(
    var_name,
    executable(
      tool,
      main,
      config_h,
      files_extra,
      export_dynamic: export_dynamic,
      link_args: link_args,
      dependencies: [
        libdnsdist_common,
        deps_extra,
      ],
    )
  )

  if 'manpages' in info
    foreach man_page: info['manpages']
      man_pages += docs_dir / 'manpages' / (man_page + '.rst')
    endforeach
  endif
endforeach

# Man-pages.
py = import('python')
python = py.find_installation('python3', modules: 'venv', required: false)

summary('Python', python.found(), bool_yn: true, section: 'Manual Pages')
summary('Path', python.full_path(), section: 'Manual Pages')
summary('Version', python.version(), section: 'Manual Pages')

if python.found()
  run_target(
    'man-pages',
    command: [
      python,
      product_source_dir / docs_dir / 'generate-man-pages.py',
      '--venv-name', 'venv-dnsdist-man-pages',
      '--requirements-file', docs_dir / 'requirements.txt',
      '--source-directory', docs_dir,
      '--target-directory', 'dnsdist-man-pages',
    ] + man_pages,
  )
endif
