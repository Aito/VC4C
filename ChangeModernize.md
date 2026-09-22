# VC4C / VC4CL Modernization Summary (LLVM 19 / GCC 14 / Debian Trixie 64-bit)

A record of modifications made to support building and executing on the latest toolchains in the Raspberry Pi 3 (aarch64) environment.

## 1. VC4C (Compiler) Modifications
- **C++17 Support**:
  - Fixed custom `Optional.h` related code to resolve ambiguity in `std::optional` comparison operators.
  - Fixed strict placement of the `[[nodiscard]]` attribute (must precede the function return type).
- **LLVM 19 Support**:
  - `BitcodeReader.cpp`: Fully migrated to Opaque Pointers and removed calls to deprecated APIs.
  - `LLVMLibrary.cpp`: Adapted to the modified `DiagnosticHandler` callback signature.
- **Clang 19 Support**:
  - `FrontendCompiler.cpp`: In Clang 19, specifying both `-S` (assembly output) and `-emit-llvm-bc` (bitcode output) simultaneously causes a compilation error. Applied a patch to drop `-S` when emitting bitcode.

## 2. VC4CL (Runtime) Modifications
- **64-bit Kernel Support**:
  - The legacy `vcsm` (Mailbox) does not work on recent 64-bit kernels. Disabled Mailbox memory allocation and migrated to `DRM (Direct Rendering Manager)` based memory allocation.
- **Enabling JIT Compilation**:
  - Enabled `HAS_COMPILER=ON` to link `libVC4CC.so`, allowing kernel compilation at runtime (`clBuildProgram`).

## 3. Runtime Architectural Constraints and Workarounds (Important)
- **Issue**: Dynamically loading (`dlopen`) VC4CL via the `libOpenCL.so` (ICD loader) causes a bus error (SIGBUS) during the TLS (Thread Local Storage) initialization of the internally linked `libLLVM.so`.
- **Cause**: Due to architectural specifications in the aarch64 Linux environment, dynamically loading a library (LLVM) compiled with the `initial-exec` model and possessing a large TLS via `dlopen` fails memory allocation.
- **Workaround**: Confirmed that execution works normally by specifying `LD_PRELOAD=/usr/local/lib/libVC4CL.so` to force loading the runtime at program startup.
