# Ash wa Azida — Native Android build

This repository keeps the lightweight browser prototype, and the `native-android` branch builds a real Android APK through GitHub Actions.

## Native build

- Android app ID: `com.ash.waazida`
- Minimum Android version: Android 11 (API 30)
- Target device ABI: `arm64-v8a` (Galaxy A70)
- Inference: upstream `llama.cpp` Android inference library; GGUF stays on the phone.
- No API key and no hosted inference service are used.
- Model selection uses Android's document picker. The upstream sample copies the selected GGUF into app-private storage before loading it.

The workflow pins llama.cpp to commit `f3586e66bc916d672773d4a6505513ba91a3595c` so a future upstream change does not silently alter this build.

## Download the APK

Open the repository's **Actions** tab, choose **Build Ash wa Azida Android APK**, open the latest successful run, and download the `ash-wa-azida-arm64-debug-apk` artifact. Extract the ZIP and install `app-debug.apk`.

## Device and model limits

Native Android avoids the browser runtime's per-file 2 GB limitation. It does **not** remove RAM requirements. On an 8 GB phone, start with a 1B–3B Q4 GGUF and a modest context. A 4.7 GB 8B GGUF may still fail or be killed by Android because model weights, KV cache, app memory, and the operating system all need RAM. Ensure there is enough free storage for the app's private copy of the selected model.

## Build source

The native inference implementation is based on the official [llama.cpp Android example](https://github.com/ggml-org/llama.cpp/tree/f3586e66bc916d672773d4a6505513ba91a3595c/examples/llama.android).
