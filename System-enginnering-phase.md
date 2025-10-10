# 🧩 System Engineering Phase Checklist — QR-Share (C/ASM)

**Project Title:** QR-Share (C/ASM) for GNU/Linux  
**Version:** 1.0.0-MVP  
**Platform:** GNU/Linux (x86-64)  
**Primary Language:** C (C99/C11)  
**Optimization:** Assembly (x86-64 SIMD: SSE/AVX)

---

## 🏗️ 1. System Requirements Phase

### ✅ Objectives
Define what the system must *do* and under what constraints.

### 📋 Checklist

#### Functional Requirements
- [ ] Detect primary local IP using `getifaddrs()`.
- [ ] Run as minimal HTTP/TCP server.
- [ ] Serve `/upload` (HTML upload page) via GET.
- [ ] Accept `/receive` via POST and store file.
- [ ] Generate and render QR code via `libqrencode`.
- [ ] Compute SHA-256 hash (optimized via Assembly SIMD).

#### Non-Functional Requirements
- [ ] Written strictly in C and Assembly.
- [ ] No external frameworks or interpreted languages.
- [ ] CLI-only, no GUI dependencies.
- [ ] Robust error handling for socket, file I/O, and network issues.

#### Platform Dependencies
- [ ] POSIX-compliant Linux system (kernel ≥ 4.0).
- [ ] GCC or Clang toolchain.
- [ ] `libqrencode` installed.

#### Security Requirements
- [ ] Operates within local network only.
- [ ] Uses a random high TCP port (≥1024).
- [ ] Prevents directory traversal (`../`).
- [ ] Verifies file integrity using SHA-256 before saving.

---

## 🧠 2. System Design Phase

### ✅ Objectives
Architect and design subsystems: networking, QR generation, hashing, CLI.

### 📋 Checklist

#### High-Level Architecture
- [ ] Define modules:
  - [ ] `network.c` — socket setup and HTTP handling.
  - [ ] `qr.c` — QR encoding and terminal rendering.
  - [ ] `sha256_asm.S` — SIMD-optimized hashing.
  - [ ] `main.c` — CLI orchestration.
- [ ] Define headers:
  - [ ] `network.h`, `qr.h`, `hash.h`, `utils.h`.

#### Data Flow
Mobile → Scan QR → Browser Upload → C Server → File Write → SHA-256 Verify → Save → Done


#### Error Handling
- [ ] Define unified error codes or struct.
- [ ] Ensure socket, memory, and file I/O checks.

#### Assembly Integration
- [ ] Use `__asm__` or `.S` files.
- [ ] Follow System V AMD64 ABI for compatibility.
- [ ] Verify inline ASM constraints.

---

## 🧱 3. Implementation Phase

### ✅ Objectives
Implement, compile, and integrate C and ASM components.

### 📋 Checklist

#### Network Server
- [ ] Create socket, bind, listen, accept connections.
- [ ] Parse HTTP headers manually.
- [ ] Handle GET `/upload` and POST `/receive`.
- [ ] Serve embedded HTML form.

#### QR Generation
- [ ] Encode `http://<IP>:<PORT>/upload`.
- [ ] Render in terminal using Unicode blocks (`U+2588`).

#### File Handling
- [ ] Stream file to temporary path.
- [ ] Calculate SHA-256 hash after upload.
- [ ] Move verified file to `~/Downloads` or `$XDG_DOWNLOAD_DIR`.

#### SHA-256
- [ ] Implement base C version for reference.
- [ ] Implement SIMD version (SSE/AVX) for performance.

#### CLI Interface
- [ ] Display QR code and upload URL.
- [ ] Show transfer progress.
- [ ] Display hash verification result.

#### Build System
- [ ] Create `Makefile`:
  - [ ] Targets: `all`, `clean`, `test`, `run`.
  - [ ] Flags: `-O3 -march=native -Wall -Wextra`.
  - [ ] Link `-lqrencode`.
- [ ] Test builds on Debian, Arch, Fedora.

---

## 🧪 4. Verification & Validation Phase

### ✅ Objectives
Validate correctness, reliability, and performance.

### 📋 Checklist

#### Unit Tests
- [ ] Test `sha256_asm()` with known test vectors.
- [ ] Test local IP detection (`getifaddrs()`).
- [ ] Test HTTP GET/POST parsing.

#### Integration Tests
- [ ] Upload via `curl -F` and verify server response.
- [ ] Validate file integrity after transfer.
- [ ] Confirm QR scan opens correct upload page.

#### Performance Tests
- [ ] Compare C vs ASM hash speed.
- [ ] Test with 10MB and 100MB files.

#### Security Tests
- [ ] Reject large uploads (>1GB).
- [ ] Sanitize filenames.
- [ ] Confirm server listens on local IP only.

#### Error Handling Tests
- [ ] Simulate network disconnects.
- [ ] Test low disk space behavior.
- [ ] Handle malformed HTTP requests gracefully.

---

## 🚀 5. Deployment Phase

### ✅ Objectives
Prepare release, packaging, and documentation.

### 📋 Checklist
- [ ] Produce static build (`-static`).
- [ ] Write `README.md` with install and usage instructions.
- [ ] Include dependencies (`libqrencode-dev`).
- [ ] Write `qr-share.1` man page.
- [ ] Package as `.deb` and `.tar.gz`.
- [ ] Install to `/usr/local/bin`.
- [ ] Test binary on multiple Linux distros.
- [ ] Confirm no root privileges required.

---

## 🛠️ 6. Maintenance & Enhancement Phase

### ✅ Objectives
Maintain reliability and extend functionality.

### 📋 Checklist
- [ ] Run static analysis (`clang-tidy`, `cppcheck`).
- [ ] Refactor for readability and modularity.
- [ ] Optimize hashing for AVX512.
- [ ] Add optional encryption (future enhancement).
- [ ] Add CLI flags (`--port`, `--dir`).
- [ ] Implement IPv6 support.
- [ ] Setup CI testing (GitHub Actions).
- [ ] Maintain `CHANGELOG.md` and semantic versioning (`v1.0.1`, `v1.0.2`).

---

## 📘 Bonus: Documentation Deliverables

| Deliverable | Description |
|--------------|--------------|
| **SYSTEM_SPEC.md** | Detailed requirements and architecture overview. |
| **DESIGN_DOC.md** | Module layout, data flow, and integration diagrams. |
| **IMPLEMENTATION_NOTES.md** | Coding standards, build flags, and memory management notes. |
| **TEST_PLAN.md** | Unit, integration, and performance test coverage. |
| **BUILD_GUIDE.md** | Steps to compile and execute the binary. |

---

## 🧭 Summary Development Flow

System Requirements → Design → Implementation → Validation → Deployment → Maintenance


---

## 📅 Optional: Recommended MVP Sprint Plan

| Phase | Duration | Deliverables |
|--------|-----------|--------------|
| Requirements | 2 days | SYSTEM_SPEC.md |
| Design | 3 days | DESIGN_DOC.md |
| Implementation | 7 days | Initial working binary |
| Validation | 3 days | Test Plan + Reports |
| Deployment | 2 days | Packaged release |
| Maintenance | Continuous | Optimizations & Updates |

---

**Author:** Shane Cardoz  
**Version:** `v1.0.0-MVP`  
**License:** MIT or GPLv3 (TBD)


