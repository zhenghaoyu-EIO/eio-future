# EIO Future — Unified Open‑Source AR Ecosystem  
[![CI](https://github.com/zhenghaoyu-EIO/eio-future/actions/workflows/ci.yml/badge.svg)](https://github.com/zhenghaoyu-EIO/eio-future/actions/workflows/ci.yml)

> **One repo · Three layers · Infinite possibilities**  
> Build for **Star1s** Android‑based AR glasses (Android 9 / API 28) and join an open marketplace where every developer can publish and earn.

---

## 1 · Project Vision

* **Open firmware** – Android 9 AOSP device tree, vendor blobs and OTA tooling.  
* **Kotlin‑first SDK** – easy access to camera, voice and core hardware helpers.  
* **Ready‑to‑fork samples** – best‑practice apps that run out of the box.  
* **Future marketplace** – one‑click distribution and revenue sharing.

All developer‑facing code is gathered in this monorepo to simplify cloning,
issue tracking and CI.

---

## 2 · Architecture at a Glance

```
                          +----------------------+
                          |  Star1s AR Glasses   |
                          |   (Android 9 / API28)|
                          +----------+-----------+
                                     ▲  OTA
           modules/eio-android-firmware ────────────────┐
                                     │                 │
                    +----------------+-----------------+
                    |     EIO ANDROID SDK (AARs)       |
                    |  core • camera • voice modules   |
                    |  modules/eio-android-sdk         |
                    +----------------+-----------------+
                                     ▲
                                     │ Gradle dependency
+-------------------------------------------------------------------+
|                  Sample Applications (modules/eio-samples)        |
|   subtitle-demo • camerax-overlay • unity-ar-template (skeleton)  |
+-------------------------------------------------------------------+
                                     ▲
                                     │ (future)
              +----------------------+------------------+
              |  Marketplace Backend (JSON index + S3)  |
              +-----------------------------------------+
```

For firmware build details (board names, flash routine, A/B OTA steps) see  
**`modules/eio-android-firmware/README.md`**.

---

## 3 · Repository Layout

| Path | What’s inside |
|------|---------------|
| `modules/eio-android-firmware/` | **Submodule** – Android 9 device tree, vendor blobs, OTA scripts |
| `modules/eio-android-sdk/` | **Submodule** – Kotlin SDK (`core`, `camera`, `voice`) |
| `modules/eio-samples/` | **Submodule** – Demo & template apps |
| `.github/workflows/ci.yml` | Minimal GitHub Actions workflow |
| `CONTRIBUTING_CN.md` | Chinese contribution guidelines |
| `LICENSE` | MIT license |

> **Why submodules?**    
> Each layer can follow its own release cadence while still sharing a single
> tracker and CI pipeline.

---

## 4 · Quick Start

### 4.1 Clone (with submodules)

```bash
git clone --recurse-submodules https://github.com/zhenghaoyu-EIO/eio-future.git
cd eio-future
```

Missed the flag? run `git submodule update --init --depth 1`.

### 4.2 Requirements

| Tool | Version | Notes |
|------|---------|-------|
| **JDK** | 17 | Temurin / OpenJDK |
| **Android SDK** | *API 28* + build‑tools 28.0.3 | `platform-tools`, `cmdline-tools/latest` |
| **Gradle Wrapper** | bundled (≥ 8.x) | No local install needed |

Device flashing steps are documented inside the firmware submodule.

### 4.3 Build everything  ⬜ TODO

Root composite build is under construction. For now:

```bash
# Build SDK AARs
cd modules/eio-android-sdk
../gradlew :core:assembleRelease :camera:assembleRelease :voice:assembleRelease

# Build sample APKs
cd ../eio-samples
../gradlew :subtitle-demo:assembleDebug :camerax-overlay:assembleDebug
```

Soon it will collapse into one command:

```bash
./gradlew assemble      # build all AARs & APKs
```

---

## 5 · Submodule Maintenance

Typical workflow to pull latest from every child:

```bash
git submodule update --remote --merge
git add modules/*
git commit -m "chore(submodules): bump pointers"
```

*Always* merge via PR in each child repo first; the monorepo only tracks
approved commits to keep history linear.

---

## 6 · Continuous Integration

The current CI does:

1. Checkout with submodules (`recursive`)  
2. Validate the Gradle wrapper  
3. Run a no‑op `./gradlew :help` to ensure the build scaffold is healthy

Future phases will add:

* Unit & instrumentation tests  
* AAR publication to GitHub Packages  
* Sample APK artifact upload  
* Static analysis (Detekt, ktlint, Lint)

---

## 7 · Contributing

See **[`CONTRIBUTING_EN.md`](CONTRIBUTING_EN.md)** for coding style and commit
rules (English version coming soon).

---

## 8 · License

Released under the **MIT License** – see [`LICENSE`](LICENSE).

---

## 9 · Contact

✉ **yu@eiofuture.ca**
