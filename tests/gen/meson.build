ed25519_donna_inc = include_directories('../ed25519-donna')
ed25519_donna_native_sta = static_library('ed25519_donna_native',
	'../ed25519-donna/ed25519.c',
	include_directories: ed25519_donna_inc,
	c_args: [
		'-DED25519_CUSTOMHASH',
		'-DED25519_TEST',
		'-DED25519_NO_INLINE_ASM',
		'-DED25519_FORCE_32BIT',
	],
	override_options: [
		'c_std=c99'
	],
	build_by_default: false,
)
ed25519_donna_native_dep = declare_dependency(
	link_with: ed25519_donna_native_sta,
	include_directories: ed25519_donna_inc,
)

vecs = [
	'hchacha20',
	'xchacha20',
	'aead_ietf',
	'blake2b',
	'sha512',
	'edDSA',
	'edDSA_pk',
	'ed_25519',
	'x25519_pk',
	'monokex_xk1',
	'monokex_x',
]

vecs_wextra = [
	'argon2i',
	'chacha20',
	'poly1305',
	'x25519',
]

vecs_nogen = [
	'ed_25519_check',
	'key_exchange',
]

vector_to_header = executable('vector_to_header',
	'../vector_to_header.c',
	native: true,
	build_by_default: false,
)

vec_h = []
foreach vec_n: vecs + vecs_wextra + vecs_nogen
	vec_files = []
	if vec_n in vecs or vec_n in vecs_wextra
		vec_gen = executable(vec_n + '-gen',
			vec_n + '.c',
			dependencies: [_sodium_native, ed25519_donna_native_dep],
			include_directories: include_directories('..'),
			native: true,
			build_by_default: false,
		)

		vec = custom_target(vec_n + '.vec',
			command: [vec_gen],
			output: vec_n + '.vec',
			capture: true,
			build_by_default: false,
		)

		vec_files += [vec]
	endif

	if vec_n in vecs_wextra or vec_n in vecs_nogen
		vec_files += [files(['../vectors/' + vec_n])]
	endif

	vec_all = custom_target(vec_n + '.all.vec',
		command: ['cat' ] + vec_files,
		output: vec_n + '.all.vec',
		capture: true,
		build_by_default: false,
	)

	vec_h += [custom_target(vec_n + '.h.vec',
		input: [vector_to_header, vec_all],
		command: ['sh', '-c', '@INPUT0@ ' + vec_n + '<@INPUT1@'],
		output: vec_n + '.h.vec',
		capture: true,
		build_by_default: false,
	)]
endforeach

vectors_h = custom_target('vectors.h',
	command: ['cat'] + vec_h,
	output: 'vectors.h',
	capture: true,
	build_by_default: false,
)
