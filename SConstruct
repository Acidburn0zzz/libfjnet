import os
import sys

environment = Environment(ENV = os.environ)

poll_api = "select"

conf = Configure(environment)

if ((conf.CheckFunc('kqueue') and conf.CheckFunc('kevent') ) or conf.CheckCHeader('sys/kqueue.h') ) and ARGUMENTS.get('poll', 'kqueue') == 'kqueue':
    environment.Append(CPPDEFINES = ["USE_KQUEUE"])
    environment.Append(CPPDEFINES = ["NEEDS_FJNET_POLL_TIMEOUT=0"])
    poll_api = "kqueue"
elif (conf.CheckFunc('poll') or conf.CheckCHeader('poll.h') ) and ARGUMENTS.get('poll', 'poll') == 'poll':
    poll_api = 'poll'
else:
    environment.Append(CPPDEFINES = ["NEEDS_FJNET_POLL_TIMEOUT=1"])

conf.Finish()

if os.name == 'posix':
    if sys.platform == 'cygwin':
        environment.Append(CPPDEFINES = ["USE_CYGSOCK", "WIN32"], CCFLAGS = " -mwindows ")
    else:
        environment.Append(CPPDEFINES = ["USE_BSDSOCK"], LIBPATH=["/usr/local/lib"])
else:
  environment.Append(LIBPATH = [os.path.join(os.getcwd(), "lib")], LIBS = ["User32.lib", "Ws2_32.lib"], CPPDEFINES = ["WIN32"])
  environment.Append(CPPDEFINES = ["USE_WINSOCK"])

SConscript(dirs = [os.getcwd()], exports = "environment poll_api")

