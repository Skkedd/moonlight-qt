# Moonlight Multi-Session

Follow the repository fleet and Tools policies. Keep this file and `GREYPRINT.md` byte-identical. Cortex owns the canonical checkout and `origin` is the Skkedd fork. Use `upstream` for read-only Moonlight integration. Preserve upstream license and third-party notices.

The Windows build uses Qt Quick and qmake for the UI, SDL for input and audio, FFmpeg for decoding, and the pinned moonlight-common-c submodule for GameStream. Start with [the ARM64 build guide](docs/windows-arm64.md) and [the architecture decision](docs/multi-session.md).

Keep Windows settings, crash dumps, logs, install paths, and upgrade identities separate from stock Moonlight. Keep development assembly and builds in the owning X Workshop record. Qualify on actual Onyx hardware before claiming native streaming acceptance. Use measured results for latency, background bandwidth and encoder/decoder behavior. Run GitHub Actions only with the required explicit approval. Production signing, promotion and public release require their own approvals.
