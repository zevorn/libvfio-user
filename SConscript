# -*- mode:python -*-

Import('env')

vfu_env = env.Clone()

# Suppress warnings from external C code in C++ build
if vfu_env['GCC'] or vfu_env['CLANG']:
    vfu_env.Append(CCFLAGS=[
        '-Wno-sign-compare',
        '-Wno-missing-field-initializers',
        '-Wno-unused-parameter',
        '-Wno-cast-function-type',
    ])

# json-c is required by libvfio-user for version negotiation
vfu_env.ParseConfig('pkg-config --cflags json-c')

vfu_env.Append(CPPPATH=[
    Dir('include').srcnode(),
    Dir('include/pci_caps').srcnode(),
    Dir('lib').srcnode(),
])

vfu_sources = [
    'lib/dma.c',
    'lib/irq.c',
    'lib/libvfio-user.c',
    'lib/migration.c',
    'lib/pci.c',
    'lib/pci_caps.c',
    'lib/tran.c',
    'lib/tran_sock.c',
]

vfu_env.Library('vfio-user',
                [vfu_env.SharedObject(File(f)) for f in vfu_sources])

# Export include path and library for consumers
env.Prepend(CPPPATH=[Dir('include').srcnode()])
env.Append(LIBS=[File('libvfio-user.a')])
env.Prepend(LIBPATH=[Dir('.')])
env.ParseConfig('pkg-config --libs json-c')
