subdir('gen')

tweetnacl_native = static_library('tweetnacl_native',
	'tweetnacl.c',
	native: true,
	build_by_default: false,
)

test_base = executable('test-base',
	'test.c', vectors_h,
	dependencies: [monocypher_native_dep, monocypher_native_opt_dep],
	native: true,
	build_by_default: false,
)
test('test', test_base)

test_speed = executable('speed',
	'speed.c', vectors_h,
	dependencies: [monocypher_native_dep, monocypher_native_opt_dep],
	native: true,
	build_by_default: false,
)
benchmark('speed', test_speed)

test_speed_tweetnacl = executable('speed-tweetnacl',
	'speed-tweetnacl.c', vectors_h,
	link_with: tweetnacl_native,
	dependencies: monocypher_native_dep,
	native: true,
	build_by_default: false,
)
benchmark('speed-tweetnacl', test_speed_tweetnacl,
	timeout: 60
)

test_speed_sodium = executable('speed-sodium',
	'speed-sodium.c', vectors_h,
	dependencies: [monocypher_native_dep, _sodium_native],
	native: true,
	build_by_default: false,
)
benchmark('speed-sodium', test_speed_sodium)
