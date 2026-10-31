# Ash wa Azida

A mobile-first, browser-based local GGUF chat app. The model runs on the device through Wllama/WebAssembly; there is no hosted inference API and no API key.

## Run it

1. Open the deployed HTTPS site: https://ashwaazida.vercel.app
2. Choose a compatible `.gguf` file.
3. Wait for the model to load, then chat. Keep the page open while it is running.
4. After the first online visit, the service worker caches the app shell and Wllama runtime files where the browser permits. The model itself is selected from the device and is not silently uploaded.

## Device and model limits

- This is a browser runtime, not a native Android app.
- Wllama's browser loading path has a 2 GB per-file limit. A larger GGUF must be split into smaller shards and all shards selected together.
- Splitting solves the per-file limit; it does **not** reduce total RAM required. A 4.7 GB 8B model is not a sensible first test on an 8 GB Galaxy A70 and may freeze or terminate the browser.
- Start with a 1B–3B Q4 model, 2,048 context, and two CPU threads. Performance depends on browser, thermal state, available RAM, and model architecture.
- The first visit needs internet to fetch the Wllama JS/WASM runtime. After those assets are cached, inference can run offline with the model file available locally. Clear browser/site data and you may need to reconnect once.
- Chat history is stored in this browser's localStorage. It is not synced between devices. Use Export chat to save a JSON copy.

## Privacy and limitations

Prompts and model inference stay in the browser. The runtime library is downloaded from jsDelivr on first use. This prototype does not have native Android permissions, background execution, phone control, voice I/O, or web research. It does not claim to perform those actions.

## Technical notes

- Static app: HTML, CSS, JavaScript modules.
- Local inference: `@wllama/wllama` 3.8.1.
- Runtime: browser WebAssembly; no server-side model inference.
- Service worker caches the same-origin app shell and Wllama CDN assets.
