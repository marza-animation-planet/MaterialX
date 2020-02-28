import excons


env = excons.MakeBaseEnv()
out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"


mtlx_opts = {}
mtlx_opts["MATERIALX_BUILD_PYTHON"] = "1" if excons.GetArgument("materialx-bind-python", 1) != 0 else "0"
mtlx_opts["MATERIALX_BUILD_VIEWER"] = "1" if excons.GetArgument("materialx-viewer", 1) != 0 else "0"
mtlx_opts["PYBIND11_PYTHON_VERSION"] = excons.GetArgument("materialx-python-ver", "2.7", str)
mtlx_opts["MATERIALX_INSTALL_PYTHON"] = "0"


prjs = []
prjs.append({"name": "MaterialX",
             "type": "cmake",
             "cmake-opts": mtlx_opts,
             "cmake-cfgs": excons.CollectFiles(["src"], patterns=["CMakeLists.txt"], recursive=True) + ["CMakeLists.txt"],
             "cmake-srcs": excons.CollectFiles(["src"], patterns=["*.cpp"], recursive=True),
             "cmake-outputs": []}) # TODO : cmake-outputs


excons.DeclareTargets(env, prjs)
