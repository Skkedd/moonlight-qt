# Windows ARM64 development build

Use a clean checkout at the exact candidate revision with recursive submodules in the owning X Workshop folder. Run development builds there because the upstream scripts write dependencies to `libs/windows` and binaries to `build`. Keep temporary archives and build caches in that record.

The baseline at `67dbfddb20d447891977d22c0354f3641e130b0a` uses Qt 6.12.0, an x64 MSVC 2022 host Qt kit and the matching Windows ARM64 cross-compiled kit. Install the Visual Studio Build Tools ARM64 C++ component (`Microsoft.VisualStudio.Component.VC.Tools.ARM64`), an SDK with ARM64 libraries, the ARM64 VC runtime files for the portable bundle, and 7-Zip. The existing upstream scripts use `vswhere`, `qmake`, `nmake` or `jom`, `windeployqt`, and MSBuild with WiX package restore.

From an x64 Visual Studio command prompt configured for `amd64_arm64`, check `where cl` and `cl /Bv`. The compiler path must end in `Hostx64\arm64\cl.exe` and its banner must say ARM64. Use the target Qt kit's `bin` first on PATH. Its qmake wrapper uses the sibling x64 kit for host tools. Set TEMP and TMP to an existing directory in the task's Workshop record before running the build.

From the root of the isolated checkout:

```bat
git submodule update --init --recursive
powershell -NoProfile -File setup-deps.ps1
scripts\build-arch.bat Release arm64
```

The dependency script requests release v20.1 from `moonlight-stream/moonlight-qt-deps`. The release publishes the following SHA-256 digests. Verify archive bytes against these values when staging dependencies for qualification.

| Asset | SHA-256 |
| --- | --- |
| Windows-ARM64.zip | `0bf846fb960052b3f4ae4a0a71436cbc24c8b90a2dca28cc3212798792568fe4` |
| Windows-x64.zip | `5fc9f15fc61d8346ec44981127a39f3fe546afec1d7bc7858218eba2e3b0c646` |

The ARM64 build creates a deployment tree at `build/deploy-arm64-release` and a standalone ARM64 MSI via WiX. The universal bundle script additionally requires an x64 build. Check `dumpbin /headers` on the application and load-time libraries and record their hashes. The application should report PE machine `AA64`. An x64 CRT file also appears in the official portable archive, so verify the actual loaded module architectures on Onyx before shipping the fork.

The Windows fork uses the Moonlight Multi-Session installer identity and its own settings. Use an isolated Workshop installation for qualification. The binary version currently carries the upstream source version for build and packaging compatibility. The fork has no assigned release version yet. Derive a pre-1.0 maturity version from verified acceptance criteria before issuing a fork release.

On Onyx, verify first launch, pairing, H.264/HEVC/AV1 as supported, FFmpeg D3D11VA decoder selection, audio output, touch, keyboard, mouse, and controller input. Record software fallback separately and capture the Windows process architecture and observed hardware decoder activity.
