import excons
from excons.tools import python
import sys

env = excons.MakeBaseEnv()


major = "1"
minor = "35"
major = "2"


bind_python = int(excons.GetArgument("materialx-bind-python", 1))
generate_docs = int(excons.GetArgument("materialx-generate-docs", 0))


cppflags = ""
include_dirs = []
lib_dirs = []
libs = []
defs = []
customs = []
out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"
incdirs = ["source"]

defs.append("MATERIALX_MAJOR_VERSION=1")
defs.append("MATERIALX_MINOR_VERSION=35")
defs.append("MATERIALX_BUILD_VERSION=2")


if sys.platform == "win32":
    cppflags += " /W4 /MP /bigobj"
else:
    cppflags += " -std=gnu++11 -Wall -Wno-ignored-qualifiers -Wno-unused-parameter -Wno-undefined-var-template"


prjs = []


prjs.append({"name": "MaterialXCore",
             "type": "staticlib",
             "alias": "materialx-core",
             "defs": defs,
             "cppflags": cppflags,
             "incdirs": incdirs,
             "srcs": excons.glob("source/MaterialXCore/*.cpp"),
             "install": {out_incdir + "/MaterialXCore": excons.Glob("source/MaterialXCore/*.h")},
             "custom": customs})


prjs.append({"name": "MaterialXFormat",
             "type": "staticlib",
             "alias": "materialx-format",
             "defs": defs,
             "cppflags": cppflags,
             "incdirs": incdirs,
             "srcs": excons.glob("source/MaterialXFormat/*.cpp") + excons.glob("source/MaterialXFormat/PugiXML/*.cpp"),
             "install": {out_incdir + "/MaterialXFormat": excons.Glob("source/MaterialXFormat/*.h")},
             "custom": customs})


prjs.append({"name": "PyMaterialX",
             "type": "dynamicmodule",
             "alias": "materialx-py",
             "ext": python.ModuleExtension(),
             "prefix": "python/MaterialX",
             "defs": defs + ["PYBIND11_PREFER_PYINT"],
             "cppflags": cppflags,
             "symvis": "default",
             "incdirs": incdirs + ["source/PyMaterialX"],
             "libdirs": [out_libdir],
             "symvis": "hidden",
             "libs": ["MaterialXCore", "MaterialXFormat"],
             "deps": ["materialx-core", "materialx-format"],
             "srcs": excons.glob("source/PyMaterialX/*.cpp"),
             "custom": [python.SoftRequire],
             "install": {excons.OutputBaseDirectory() + "/python/MaterialX": excons.Glob("python/MaterialX/*.py")}})


excons.DeclareTargets(env, prjs)