import sys
import os
import re
import excons


env = excons.MakeBaseEnv()
out_base = excons.OutputBaseDirectory()
out_incdir = "{}/include".format(out_base)
out_libdir = "{}/lib".format(out_base)

bind_python = excons.GetArgument("materialx-bind-python", 1) != 0
build_viewer = excons.GetArgument("materialx-viewer", 1) != 0

mtlx_opts = {}
mtlx_opts["MATERIALX_BUILD_PYTHON"] = "1" if bind_python != 0 else "0"
mtlx_opts["MATERIALX_BUILD_VIEWER"] = "1" if build_viewer else "0"
mtlx_opts["PYBIND11_PYTHON_VERSION"] = excons.GetArgument("materialx-python-ver", "2.7", str)
mtlx_opts["MATERIALX_INSTALL_PYTHON"] = "0"


def MaterialXNames(names=None):
    namefunc = lambda x: "lib" + x + ".a" if sys.platform != "win32" else x + ".dll"
    libs = []

    for n in os.listdir("source"):
        fp = os.path.abspath("source/{}".format(n))
        if not os.path.isdir(fp):
            continue

        if not n.startswith("MaterialX") or n in ["MaterialXView", "MaterialXTest"]:
            continue

        if names and n not in names:
            continue

        libs.append(n)

    return map(namefunc, libs)

def MaterialXPaths(names=None):
    paths = []
    for n in MaterialXNames(names):
        paths.append(os.path.abspath(os.path.join(out_libdir, n)))

    return paths


outputs = []
for h in excons.CollectFiles(["source"], patterns=["*.h*"], recursive=True, exclude=["NanoGUI", "MaterialXTest", "PyMaterialX", "MaterialXView"]):
    outputs.append(os.path.join(out_incdir, "/".join(re.split(r"/|\\", h)[1:])))

if build_viewer:
    outputs.append("{}/bin/MaterialXView{}".format(out_base, (".exe" if sys.platform == "win32" else "")))

outputs.extend(MaterialXPaths())

if bind_python:
    pyext = ".so" if sys.platform != "win32" else ".pyd"
    for p in excons.CollectFiles(["python/MaterialX"], patterns=["*.py"], recursive=True, exclude=["NanoGUI", "MaterialXTest", "PyMaterialX", "MaterialXView"]):
        outputs.append(os.path.abspath(os.path.join(out_base, p)))

    for n in os.listdir("source/PyMaterialX"):
        fp = os.path.abspath("source/PyMaterialX/{}".format(n))
        if not os.path.isdir(fp):
            continue

        if not n.startswith("PyMaterialX"):
            continue

        outputs.append(os.path.abspath(os.path.join(out_base, "python/MaterialX", n + pyext)))


prjs = []
prjs.append({"name": "MaterialX",
             "type": "cmake",
             "cmake-opts": mtlx_opts,
             "cmake-cfgs": excons.CollectFiles(["source"], patterns=["CMakeLists.txt"], recursive=True) + ["CMakeLists.txt"],
             "cmake-srcs": excons.CollectFiles(["source"], patterns=["*.cpp"], recursive=True),
             "cmake-outputs": outputs})


excons.DeclareTargets(env, prjs)
