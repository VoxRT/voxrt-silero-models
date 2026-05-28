# VoxRT Silero VAD model weights

Pre-compiled Silero VAD model weights in **`.vxrt`** format, ready to be consumed by the [VoxRT runtime](https://github.com/VoxRT/voxrt-silero-ios) on iOS and Android.

This repo is a thin distribution layer — the actual files live as GitHub Release attachments per version, not in the git tree. The README below is the index.

---

## What is a `.vxrt` file?

A self-contained, binary, framework-agnostic model file:

- **Topology + weights** encoded in a compact format (no ONNX / PyTorch dependency at consume time).
- **AES-256-GCM encrypted** at rest, decrypted on load by the runtime ([ADR-0023](https://github.com/VoxRT/voxrt-silero-ios#license)).
- **Versioned** — newer `.vxrt` files may add encoded features that older runtimes refuse to load; check the version compatibility table below.

You don't need to know the format to use it — feed the bytes to `VoxrtSileroVad` (iOS) / `VoxrtSileroVadEngine` (Android) and it Just Works.

## Downloads

### v0.1.3

| File | Size | SHA-256 |
| ---- | ---- | ------- |
| [`silero_vad.vxrt`](https://github.com/VoxRT/voxrt-silero-models/releases/download/v0.1.3/silero_vad.vxrt) | 1.2M | `0fe8498c9bd1ae119bcb0c75c8481b3a8b8be0f95c14f334d469851c19054156` |

**Compatible with:** `VoxRT/voxrt-silero-ios@v0.1.3`, `VoxRT/voxrt-silero-android@v0.1.3`.

## How to use

```bash
curl -L -o silero_vad.vxrt \
  https://github.com/VoxRT/voxrt-silero-models/releases/download/v0.1.3/silero_vad.vxrt
# verify checksum
echo "0fe8498c9bd1ae119bcb0c75c8481b3a8b8be0f95c14f334d469851c19054156  silero_vad.vxrt" | shasum -a 256 -c
```

Then bundle (in `assets/` for Android, in Xcode resources for iOS) or download at runtime — see the platform repos for code examples.

## Provenance

The encoded weights are derived from the [Silero VAD v5](https://github.com/snakers4/silero-vad) ONNX model published by Silero Team, ported into the `.vxrt` format by VoxRT's `voxrt-compile` tool. No fine-tuning, no architectural changes — purely a re-packaging into a runtime-native format.

The original Silero VAD weights are MIT-licensed; this re-packaging retains the same license (see `LICENSE`). The VoxRT runtime that consumes the file is proprietary, distributed under a separate license — but the model file itself is free for any use.

## Versioning

A model release at `vX.Y.Z` is built to work with VoxRT runtime `vX.Y.Z` and (typically) all earlier minor versions sharing the same major. Breaking format changes bump the major. See the [iOS](https://github.com/VoxRT/voxrt-silero-ios/releases) and [Android](https://github.com/VoxRT/voxrt-silero-android/releases) release notes for the exact compatibility per release.

## Links

- iOS runtime: [voxrt-silero-ios](https://github.com/VoxRT/voxrt-silero-ios)
- Android runtime: [voxrt-silero-android](https://github.com/VoxRT/voxrt-silero-android)
- VoxRT and the runtime story: [voxrt.com](https://voxrt.com)
