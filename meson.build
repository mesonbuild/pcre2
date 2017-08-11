project('pcre2', 'c', version: '10.23', license: 'BSD-3')

pcre2_h = configure_file(input : 'src/pcre2.h.generic',
  output : 'pcre2.h',
  configuration : configuration_data())

chartables = configure_file(input : 'src/pcre2_chartables.c.dist',
  output : 'pcre2_chartables.c',
  configuration : configuration_data())


config_h = configure_file(input : 'src/config.h.generic',
  output : 'config.h',
  configuration : configuration_data())

sources = [
  'src/pcre2_auto_possess.c',
  chartables,
  config_h, pcre2_h,
  'src/pcre2_compile.c',
  'src/pcre2_config.c',
  'src/pcre2_context.c',
  'src/pcre2_dfa_match.c',
  'src/pcre2_error.c',
  'src/pcre2_find_bracket.c',
  'src/pcre2_jit_compile.c',
  'src/pcre2_maketables.c',
  'src/pcre2_match.c',
  'src/pcre2_match_data.c',
  'src/pcre2_newline.c',
  'src/pcre2_ord2utf.c',
  'src/pcre2_pattern_info.c',
  'src/pcre2_serialize.c',
  'src/pcre2_string_utils.c',
  'src/pcre2_study.c',
  'src/pcre2_substitute.c',
  'src/pcre2_substring.c',
  'src/pcre2_tables.c',
  'src/pcre2_ucd.c',
  'src/pcre2_valid_utf.c',
  'src/pcre2_xclass.c'
]

includes = [include_directories('.'), include_directories('src')]

config_h_defs = ['-DHAVE_INTTYPES_H', '-DHAVE_STRERROR',
           '-DHAVE_LIMITS_H', '-DHAVE_MEMMOVE', '-DHAVE_STDINT_H', '-DHAVE_STDLIB_H',
           '-DHAVE_STRING_H', '-DSTDC_HEADERS', '-DSUPPORT_PCRE2_8', '-DHAVE_UNISTD_H', '-DSUPPORT_UNICODE']



pcre2_8_lib = library('pcre2-8', sources,
  include_directories: includes,
  c_args: config_h_defs + ['-DHAVE_CONFIG_H', '-DPCRE2_CODE_UNIT_WIDTH=8'],
  install: true)

pcre2_posix_lib = library('pcre2-posix', ['src/pcre2posix.c', 'src/pcre2posix.h'],
  include_directories: includes,
  link_with: pcre2_8_lib,
  c_args: config_h_defs + ['-DHAVE_CONFIG_H', '-DPCRE2_CODE_UNIT_WIDTH=8'],
  install: true)

pcre2_16_lib = library('pcre2-16', sources,
  include_directories: includes,
  c_args: config_h_defs + ['-DHAVE_CONFIG_H', '-DPCRE2_CODE_UNIT_WIDTH=16'],
  install: true)

pcre2_32_lib = library('pcre2-32', sources,
  include_directories: includes,
  c_args: config_h_defs + ['-DHAVE_CONFIG_H', '-DPCRE2_CODE_UNIT_WIDTH=32'],
  install: true)

libpcre2_8 = declare_dependency(link_with: pcre2_8_lib,
  include_directories: includes)
libpcre2_posix = declare_dependency(link_with: pcre2_posix_lib,
  include_directories: includes)
libpcre2_16 = declare_dependency(link_with: pcre2_16_lib,
  include_directories: includes)
libpcre2_32 = declare_dependency(link_with: pcre2_32_lib,
  include_directories: includes)


pcre2test = executable('pcre2test', ['src/pcre2test.c'],
  include_directories: includes,
  link_with: [pcre2_8_lib, pcre2_posix_lib, pcre2_16_lib, pcre2_32_lib],
  c_args: config_h_defs + ['-DHAVE_CONFIG_H'])

pcre2grep = executable('pcre2grep', ['src/pcre2grep.c'],
  include_directories: includes,
  link_with: [pcre2_8_lib],
  c_args: config_h_defs + ['-DHAVE_CONFIG_H'],
  install: true)

########### pkg-config #############

pkg = import('pkgconfig')

pkg.generate(libraries : pcre2_8_lib,
             name : 'libpcre2-8',
             description: 'PCRE2 - Perl compatible regular expressions C library (2nd API) with 8 bit character support',
             version: meson.project_version())

pkg.generate(libraries : pcre2_16_lib,
             name : 'libpcre2-16',
             description: 'PCRE2 - Perl compatible regular expressions C library (2nd API) with 16 bit character support',
             version: meson.project_version())

pkg.generate(libraries : pcre2_32_lib,
             name : 'libpcre2-32',
             description: 'PCRE2 - Perl compatible regular expressions C library (2nd API) with 32 bit character support',
             version: meson.project_version())


#### tests

runtest = find_program('RunTest')
test('RunTest', runtest,
  env : ['srcdir=@0@'.format(meson.current_source_dir())],
  workdir: meson.current_build_dir())

