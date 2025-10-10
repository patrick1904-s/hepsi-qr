- [ ] Initialize `README.md` with project overview and usage.
- [ ] Add `LICENSE` (MIT or GPLv3).
- [ ] Setup compiler flags and build targets in `Makefile`.
- [ ] Add project metadata in `docs/PROJECT_OVERVIEW.md`.
- [ ] Create symbolic link or alias for testing (`make run`).

---

## ⚙️ Stage 2: Environment Setup

### ✅ Objectives
Prepare compiler, dependencies, and libraries.

### 📋 Checklist
- [ ] Install dependencies:
- [ ] GCC/Clang toolchain.
- [ ] `libqrencode-dev` for QR generation.
- [ ] `make` and `pkg-config`.
- [ ] Verify architecture:
- [ ] Confirm `x86-64` support.
- [ ] Check for SIMD extensions (SSE/AVX) using `lscpu`.
- [ ] Setup virtual test environment:
- [ ] Test on Debian/Ubuntu VM.
- [ ] Setup test directory `/tmp/qrshare/`.
- [ ] Configure environment variables:
- [ ] `XDG_DOWNLOAD_DIR` fallback to `~/Downloads/`.

---

## 🧩 Stage 3: Core Module Implementation

### ✅ Objectives
Implement essential functionality in isolated modules.

### 📋 Checklist

#### 1️⃣ Network Module (`network.c`)
- [ ] Implement socket creation (`socket()`, `bind()`, `listen()`).
- [ ] Auto-detect local IP (`getifaddrs()`).
- [ ] Implement minimal HTTP request parser.
- [ ] Handle routes:
- [ ] `GET /upload` → Serve HTML form.
- [ ] `POST /receive` → Handle file upload.
- [ ] Implement response headers (`Content-Type`, `Content-Length`).
- [ ] Close sockets cleanly on interrupt.

#### 2️⃣ QR Module (`qr.c`)
- [ ] Use `libqrencode` to encode URL.
- [ ] Render QR in terminal using Unicode block characters.
- [ ] Display connection URL below QR.

#### 3️⃣ File Handling Module (`hash.c`)
- [ ] Handle multipart/form-data parsing manually.
- [ ] Write uploaded file to temp directory.
- [ ] Move verified file to final storage (`~/Downloads`).
- [ ] Handle file size validation.

#### 4️⃣ SHA-256 Optimization (`sha256_asm.S`)
- [ ] Implement base C SHA-256.
- [ ] Implement SIMD version with SSE/AVX.
- [ ] Benchmark hash speed vs C version.
- [ ] Provide fallback if ASM unsupported.

#### 5️⃣ CLI Module (`main.c`)
- [ ] Initialize modules sequentially.
- [ ] Display QR, URL, and status.
- [ ] Handle SIGINT for graceful shutdown.

---

## 🧪 Stage 4: Testing & Verification

### ✅ Objectives
Ensure correctness, performance, and reliability.

### 📋 Checklist

#### Unit Tests (`tests/unit_tests.c`)
- [ ] Test `sha256_asm()` against known vectors.
- [ ] Test `getifaddrs()` returns valid IP.
- [ ] Test HTTP parsing (valid & invalid requests).

#### Integration Tests
- [ ] Upload file using `curl -F "file=@test.txt"`.
- [ ] Verify file saved in correct directory.
- [ ] Validate QR scan redirects correctly.
- [ ] Confirm hash displayed matches computed value.

#### Security Tests
- [ ] Test for directory traversal attacks.
- [ ] Limit upload file size.
- [ ] Ensure server binds only to local IP range.

#### Performance Tests
- [ ] Benchmark hash performance (`time ./qr-share testfile`).
- [ ] Measure upload latency for 10MB and 100MB files.

---

## 🚀 Stage 5: Packaging & Deployment

### ✅ Objectives
Make the project portable, installable, and easy to use.

### 📋 Checklist
- [ ] Build static binary (`make static`).
- [ ] Create `.deb` and `.tar.gz` packages.
- [ ] Verify install path: `/usr/local/bin/qr-share`.
- [ ] Create `man` page (`qr-share.1`).
- [ ] Add usage examples in `README.md`:
```bash
$ ./qr-share
QR-Share v1.0.0
Scan the QR code to upload your file.

🔄 Stage 6: Documentation & Versioning
✅ Objectives

Provide detailed developer and user documentation.

