project(
	'Monocypher',
	'c',
	license: 'BSD2 || CC0',
	version: '2.0.5',
	meson_version : '>= 0.49.0'
)

_sodium_native = dependency('libsodium', native: true, disabler: true)

_build_by_default = not meson.is_subproject()
_mc_inc = include_directories('src')
libmonocypher = both_libraries('monocypher',
	'src/monocypher.c',
	install: true,
	build_by_default: _build_by_default,
)

monocypher_dep = declare_dependency(
	link_with: libmonocypher,
	include_directories: _mc_inc
)

libmonocypher_sta_native = static_library('monocypher_native',
	'src/monocypher.c',
	native: true,
	build_by_default: _build_by_default,
)

monocypher_native_dep = declare_dependency(
	link_with: libmonocypher_sta_native,
	include_directories: _mc_inc,
)

_mc_optional_inc = include_directories('src/optional')
_mc_sha512 = static_library('monocypher_sha512_native',
	'src/optional/sha512.c',
	native: true,
	build_by_default: _build_by_default,
)
monocypher_native_opt_dep = declare_dependency(
	link_with: _mc_sha512,
	include_directories: _mc_optional_inc,
)

install_headers('src/monocypher.h')

if not meson.is_subproject()
	subdir('tests')
endif
