# -*- mode: python; -*-
import os
import os.path

vars = Variables(None, ARGUMENTS)
vars.Add(PathVariable('MONGO_SRC_DIR',
                         'Path to mongodb source directory',
                         None,
                         PathVariable.PathIsDir))
vars.Add(PathVariable('MONGO_PREFIX',
                         'Path to mongodb prefix directory',
                         None,
                         PathVariable.PathIsDir))
env = Environment(variables = vars)
Help(vars.GenerateHelpText(env))
if 'MONGO_SRC_DIR' not in env:
    print("Error: no MONGO_SRC_DIR specified")
    Exit(-1)
if 'MONGO_PREFIX' not in env:
    print("Error: no MONGO_PREFIX specified")
    Exit(-1)

env.Append(CPPFLAGS=['-pthread', '-O3', '-g'])
env.Append(LINKFLAGS=['-pthread', '-g'])

if 'darwin' == os.sys.platform:
    env.Append(CPPPATH=['/opt/local/include'])
    env.Append(LIBPATH=['/opt/local/lib'])

env.Append(CPPPATH=['mongo-cxx-driver/src'])
env.Append(CPPPATH=['mongo-cxx-driver/src/mongo'])
env.Append(LIBPATH=['mongo-cxx-driver'])
env.Append(LIBPATH=['mongo-cxx-driver'])
env.Append(LIBPATH=[os.path.join(env['MONGO_SRC_DIR'], '/build/darwin/normal/third_party/boost/')])
env.Append(LIBPATH=[os.path.join(env['MONGO_PREFIX'], 'lib')])
env.Append(CPPPATH=[os.path.join(env['MONGO_PREFIX'], 'include')])

conf = Configure( env )
libs = [ "mongoclient",  "boost_thread" , "boost_filesystem" , 'boost_program_options', 'boost_system']

def checkLib( lib ):
    if lib.startswith('boost_'):
        if conf.CheckLib( lib ):
            return True

    if conf.CheckLib( lib ):
        return True

    print( "Error: can't find library: " + str( lib ) )
    Exit(-1)
    return False

for x in libs:
    checkLib( x )

env = conf.Finish()

env.Program( "benchmark" , ["benchmark.cpp"] )
