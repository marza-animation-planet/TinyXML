import sys
import glob
import excons

env = excons.MakeBaseEnv()

out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"

use_stl = (excons.GetArgument("tinyxml-use-stl", 1, int) != 0)

def TinyXmlName():
   return "tinyxml" + excons.GetArgument("tinyxml-suffix", "")

def TinyXmlPath():
   name = TinyXmlName()
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + ".a"
   return out_libdir + "/" + libname

def RequireTinyXml(env):
   if use_stl:
      env.Append(CPPDEFINES=["TIXML_USE_STL"])
   env.Append(CPPPATH=[out_incdir])
   env.Append(LIBPATH=[out_libdir])
   excons.Link(env, TinyXmlName(), static=True, force=True, silent=True)


prjs = [
   {  "name": TinyXmlName(),
      "alias": "tinyxml",
      "type": "staticlib",
      "symvis": "default",
      "defs": (["TIXML_USE_STL"] if use_stl else []),
      "srcs": ["tinystr.cpp",
               "tinyxmlerror.cpp",
               "tinyxmlparser.cpp",
               "tinyxml.cpp"],
      "install": {"include": glob.glob("*.h")}
   }
]

excons.AddHelpOptions(tinyxml="""TINYXML OPTIONS
  tinyxml-suffix=<str> : Library name suffix                            []
  tinyxml-use-stl=0|1  : Toggle use of C++ STL                          [1]""")

excons.DeclareTargets(env, prjs)


Export("TinyXmlName TinyXmlPath RequireTinyXml")

