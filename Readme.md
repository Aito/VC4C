# VC4C (Modernized Fork)

**VC4C** is a compiler for the VideoCore IV GPU (found in Raspberry Pi 1, 2, 3, and Zero), designed to translate OpenCL C kernels into QPU machine code.

This repository is a **modernized fork** of the original VC4C project. It has been extensively updated to support modern compilation environments (such as Raspberry Pi OS Trixie, Linux 6.x kernels, GCC 14, and LLVM 19), eliminating legacy dependencies and ensuring it remains usable and maintainable for modern Raspberry Pi deployments.

## 🚀 Key Modernization & Features

- **Target Platform**: Raspberry Pi (VideoCore IV)
- **Modern Environment Support**: 
  - Fully compatible with the latest **Raspberry Pi OS (Trixie)** and **Linux 6.x** kernel environments.
  - Adapted to modern C++ standards (C++17) and built seamlessly with **GCC 14**.
  - Fully integrated with **LLVM 19**, successfully migrating from legacy APIs to opaque pointers and modern DiagnosticHandler signatures.
- **Legacy Free**: 
  - Eliminated outdated dependencies on legacy Broadcom proprietary APIs (`bcm_host`, `vcsm`, `mailbox`, `VCHI`), ensuring standalone operation.
- **Clang 19 Compatibility**:
  - Implemented strict command-line argument fixes required by Clang 19 (e.g., proper handling of `-emit-llvm-bc` and `-S` during bitcode emission).
- **Verified on Hardware**:
  - Successfully tested on actual Raspberry Pi hardware. Compiles custom OpenCL kernels (e.g., heavy math loops, vector additions) and achieves massive speedups compared to CPU execution.

## 📦 Build and Installation

### Prerequisites
Ensure you have the latest build tools installed on your Raspberry Pi:
```bash
sudo apt update
sudo apt install cmake gcc g++ clang llvm-dev libclang-dev spirv-tools
```

### Build Instructions
```bash
git clone <your_repository_url>/VC4C.git
cd VC4C

# Create build directory and configure with CMake
cmake -B build -DCMAKE_BUILD_TYPE=Release

# Build using multiple cores
make -C build -j4

# Install to the system (/usr/local/bin, /usr/local/lib, /usr/local/include)
sudo cmake --install build
```
Once installed, the compiler (`vc4c`) will be available globally, and its shared library (`libVC4CC.so`) will be ready to be linked by the **VC4CL** runtime for JIT compilation.

## 🔗 Related Project
To actually execute the compiled OpenCL kernels on your Raspberry Pi, you must also install the modernized OpenCL runtime: **[VC4CL](<your_vc4cl_repository_url>)**.

## 🛠️ Development & Modernization Environment
This modernization project was successfully completed with the assistance of **Antigravity 2.0 (Powered by Gemini 3.1 Pro)**.

The development, debugging, and verification were conducted under the following environment:
- **AI Assistant**: Antigravity 2.0 + Gemini 3.1 Pro (Google Deepmind)
- **Host Machine**: macOS
- **Target Hardware**: Raspberry Pi 3 Model B (aarch64)
- **Target OS**: Raspberry Pi OS (Trixie)
- **Target Kernel**: Linux 6.x
- **Toolchain**: GCC 14.2.0, LLVM 19.1.7, Clang 19
