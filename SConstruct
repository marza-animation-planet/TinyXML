import sys
import glob
import excons
import SCons.Script # pylint: disable=import-error


env = excons.MakeBaseEnv()

out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"

use_stl = (excons.GetArgument("tinyxml-use-stl", 1, int) != 0)

def TinyXmlName():
   name = "tinyxml" + excons.GetArgument("tinyxml-suffix", "")
   if sys.platform == "win32":
      name = "lib" + name
   return name

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
   excons.Link(env, TinyXmlPath(), static=True, force=True, silent=True)


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


SCons.Script.Export("TinyXmlName TinyXmlPath RequireTinyXml")

