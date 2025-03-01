ma_extras_c_args = ma_c_args
if cc.get_argument_syntax() == 'msvc'
  ma_extras_c_args += '/FIminiaudio.h'
else
  ma_extras_c_args += ['-include', 'miniaudio.h']
endif

ma_extra_srcs = []

opus_dep = dependency('opus', required: get_option('libopus'))
opusfile_dep = dependency('opusfile', required: get_option('libopus'))
if opus_dep.found() and opusfile_dep.found()
  ma_all_libs += static_library(
    'miniaudio_libopus',
    configure_file(
      input: 'miniaudio_libopus.h',
      output: 'miinaudio_libopus.c',
      copy: true,
    ),
    extra_files: 'miniaudio_libopus.h',
    dependencies: ma_sys_deps + [opus_dep, opusfile_dep],
    c_args: ma_extras_c_args,
    install: false,
    implicit_include_directories: false,
    include_directories: ma_inc,
    pic: true,
  )
endif

vorbis_dep = dependency('vorbis', required: get_option('libvorbis'))
vorbisfile_dep = dependency('vorbisfile', required: get_option('libvorbis'))
if vorbis_dep.found() and vorbisfile_dep.found()
  ma_all_libs += static_library(
    'miniaudio_libvorbis',
    configure_file(
      input: 'miniaudio_libvorbis.h',
      output: 'miinaudio_libvorbis.c',
      copy: true,
    ),
    extra_files: 'miniaudio_libvorbis.h',
    dependencies: ma_sys_deps + [vorbis_dep, vorbisfile_dep],
    c_args: ma_extras_c_args,
    install: false,
    implicit_include_directories: false,
    include_directories: ma_inc,
    pic: true,
  )

  if get_option('stbvorbis')
    warning(
      'you might want to disable the built-in stb_vorbis decoder to ensure the libvorbis decoder is picked.',
    )
  endif
endif

if get_option('stbvorbis')
  stbvorbis_file = files('stb_vorbis.c')
  ma_all_libs += static_library(
    'miniaudio_stbvorbis',
    stbvorbis_file,
    dependencies: ma_sys_deps,
    c_args: ma_c_args,
    install: false,
    pic: true,
  )

  ma_extra_srcs += stbvorbis_file
  ma_c_args += '-DSTB_VORBIS_HEADER_ONLY'
else
  ma_c_args += '-DMA_NO_VORBIS'
endif

if node_graph.enabled()
  subdir('nodes')
endif
