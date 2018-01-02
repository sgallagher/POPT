project('popt', 'c',
        version : '1.16',
        default_options : [
          'buildtype=debugoptimized',
          'c_std=gnu99',
          'warning_level=1',
          'b_asneeded=true',
        ],
        license : 'MIT',
        meson_version : '>=0.36.0')

cc = meson.get_compiler('c')
test_cflags = [
'-Wall',
'-pedantic',
'-Wextra',
'-Wformat-y2k',
'-fdiagnostics-show-option',
'-funit-at-a-time',
'-fstrict-aliasing',
'-Wstrict-overflow',
'-fstrict-overflow',
'-Wpointer-arith',
'-Wundef',
'-Wformat-security',
'-Winit-self',
'-Wmissing-include-dirs',
'-Wunused',
'-Wunknown-pragmas',
'-Wstrict-aliasing',
'-Wshadow',
'-Wbad-function-cast',
'-Wcast-align',
'-Wwrite-strings',
'-Wlogical-op',
'-Waggregate-return',
'-Wstrict-prototypes',
'-Wold-style-definition',
'-Wmissing-prototypes',
'-Wmissing-declarations',
'-Wmissing-noreturn',
'-Wmissing-format-attribute',
'-Wredundant-decls',
'-Wnested-externs',
'-Winline',
'-Winvalid-pch',
'-Wvolatile-register-var',
'-Wdisabled-optimization',
'-Wbuiltin-macro-redefined',
'-Wpacked-bitfield-compat',
'-Wsync-nand',
'-Wattributes',
'-Wcoverage-mismatch',
'-Wmultichar',
'-Wcpp',
'-Wdeprecated-declarations',
'-Wdiv-by-zero',
'-Wdouble-promotion',
'-Wendif-labels',
'-Wformat-contains-nul',
'-Wformat-extra-args',
'-Wformat-zero-length',
'-Wformat=2',
'-Wmultichar',
'-Wnormalized=nfc',
'-Woverflow',
'-Wpointer-to-int-cast',
'-Wpragmas',
'-Wsuggest-attribute=const',
'-Wsuggest-attribute=noreturn',
'-Wsuggest-attribute=pure',
'-Wtrampolines',
'-Wno-missing-field-initializers',
'-Wno-sign-compare',
'-Wjump-misses-init',
'-Wno-format-nonliteral',
'-Wframe-larger-than=32768',
'-fstack-protector-all',
'-fasynchronous-unwind-tables',
'-fdiagnostics-show-option',
'-funit-at-a-time',
'-fipa-pure-const',
'-Wno-aggregate-return',
'-Wno-redundant-decls',
'-Wdeclaration-after-statement',
'-DHAVE_CONFIG_H',
]

foreach cflag: test_cflags
  if cc.has_argument(cflag)
    add_project_arguments(cflag, language : 'c')
  endif
endforeach

libpopt_srcs = [
    'popt.c',
    'poptparse.c',
    'poptconfig.c',
    'popthelp.c',
    'poptint.c',
]

libpopt_lib = static_library(
    'libpopt',
    sources : libpopt_srcs,
    install : false,
    pic : true,
)

cdata = configuration_data()
cdata.set_quoted('version', meson.project_version())
cdata.set_quoted('PREFIX', get_option('prefix'))
cdata.set_quoted('EXEC_PREFIX', get_option('prefix'))

dirs = [
    'libexecdir',
    'datadir',
    'datarootdir',
    'sysconfdir',
    'sharedstatedir',
    'localstatedir',
    'includedir',
    'oldincludedir',
    'docdir',
    'infodir',
    'libdir',
    'localedir',
    'mandir',
    'bindir',
    'sbindir',
]

pkgdirs = [
    'docdir',
    'htmldir',
    'dvidir',
    'psdir',
    'pdfdir',
    'pkgdatadir',
    'pkgincludedir',
    'pkglibdir',
    'pkglibexecdir',
    'popt_alias',
]


foreach d : dirs
    if get_option(d).startswith('/')
        cdata.set_quoted(d.to_upper(), get_option(d))
    else
        if get_option(d) != ''
            cdata.set_quoted(
                d.to_upper(),
                get_option('prefix') + '/' + get_option(d))
        endif
    endif
endforeach

foreach d : pkgdirs
    if get_option(d).startswith('/')
        cdata.set_quoted(d.to_upper(), get_option(d))
    else
        if get_option(d) != ''
            cdata.set_quoted(
                d.to_upper(),
                get_option('prefix') + '/' + get_option(d) + '/' + meson.project_name())
        endif
    endif
endforeach

stdlib_funcs = [
    'access',
    'strchr',
    'getuid',
    'geteuid',
    'iconv',
    'mtrace',
    '__secure_getenv',
    'secure_gentenv',
    'setregid',
    'stpcpy',
    'strerror',
    'vasprintf',
    'srandom',
    'malloc',
    'realloc',
    'strtod',
    'memset',
    'nl_langinfo',
    'strchr',
    'strerror',
    'strrchr',
]

foreach fn : stdlib_funcs
    if cc.has_function(fn)
        cdata.set('HAVE_' + fn.to_upper(), 1)
    else
        cdata.set('HAVE_' +fn.to_upper(), false)
    endif
endforeach

system_hdrs = [
    'alloca',
    'assert',
    'dlfcn',
    'fcntl',
    'float',
    'fnmatch',
    'glob',
    'inttypes',
    'langinfo',
    'libintl',
    'limits',
    'memory',
    'stdint',
    'stdlib',
    'strings',
    'string',
    'sys_ioctl',
    'sys_stat',
    'sys_types',
    'unistd',
    'wchar',
]

foreach hdr : system_hdrs
    if cc.has_header(hdr + '.h')
        cdata.set('HAVE_' + hdr.to_upper() + '_H', true)
    else
        cdata.set('HAVE_' + hdr.to_upper() + '_H', false)
    endif
endforeach

cdata.set_quoted('PACKAGE', meson.project_name())

configure_file(
    output : 'configmake.h',
    configuration : cdata)

configure_file(
    output : 'config.h',
    configuration : cdata)

libpopt_a = declare_dependency(
                link_with : libpopt_lib,
                include_directories : include_directories('.'))
