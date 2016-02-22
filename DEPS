vars = {
  'chromium_git': 'https://chromium.googlesource.com',
}

use_relative_paths = True

deps = {
  'buildtools':
    Var('chromium_git') + '/chromium/buildtools.git' + '@' + '3b302fef93f7cc58d9b8168466905237484b2772',

  'tools/gyp':
    Var('chromium_git') + '/external/gyp.git' + '@' + '2889664b9fa88cce175c5c7cdf207d28420a7412',
}

deps_os = {
  'android': {
    'third_party/jsr-305/src':
      Var('chromium_git') + '/external/jsr-305.git' + '@' + '642c508235471f7220af6d5df2d3210e3bfc0919',
  },
  'win': {
    'testing/gtest':
      Var('chromium_git') + '/external/googletest.git' + '@' + 'be1868139ffe0ccd0e8e3b37292b84c821d9c8ad',

    'third_party/icu':
      Var('chromium_git') + '/chromium/deps/icu.git' + '@' +'e4c31439828d356525b71ef81a6d61ea50d7d673',
  },
}

hooks = [
  # Pull GN binaries.
  {
    'name': 'gn_win',
    'pattern': '.',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=win32',
                '--no_auth',
                '--bucket', 'chromium-gn',
                '-s', 'src/buildtools/win/gn.exe.sha1',
    ],
  },
  {
    'name': 'gn_linux64',
    'pattern': '.',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=linux*',
                '--no_auth',
                '--bucket', 'chromium-gn',
                '-s', 'src/buildtools/linux64/gn.sha1',
    ],
  },
  {
    # Update the Windows toolchain if necessary.
    'name': 'win_toolchain',
    'pattern': '.',
    'action': ['python', 'src/build/vs_toolchain.py', 'update'],
  },
  {
    # Pull clang if needed or requested via GYP_DEFINES.
    # Note: On Win, this should run after win_toolchain, as it may use it.
    'name': 'clang',
    'pattern': '.',
    'action': ['python', 'src/tools/clang/scripts/update.py', '--if-needed'],
  },
]
