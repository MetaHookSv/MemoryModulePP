# CMake build

This component starts from the MemoryModulePP source vendored in MetaHookSv
commit `11a852774b1725d02735aeb348c32a7bf454507c`. C++ sources and headers are
unchanged; the CMake target replaces the workspace-specific MSBuild integration.

The supported integration is `add_subdirectory(thirdparty/MemoryModulePP)`
inside the prepared MetaHook workspace. Target `MemoryModule` is an x86 MSVC
static library, exports its `include` directory, uses C++20 and static CRT
(`/MTd` in Debug, `/MT` in Release), and links `ntdll`. The parent workspace
applies VC-LTL consistently to the launcher and its dependencies.

The repository origin is `https://github.com/MetaHookSv/MemoryModulePP`.
The initial commit has only been prepared locally; publish it before expecting
a remote recursive clone of the parent workspace to succeed.
