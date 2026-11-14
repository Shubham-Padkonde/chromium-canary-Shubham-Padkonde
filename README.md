# Chromium Canary — Shubham Padkonde

> **Submission status: INCOMPLETE / UNVERIFIED.** This repository currently includes a proposed Chromium patch and GN configuration, **not a successfully compiled Chromium build**. The live demonstration video, genuine AI logs, and hardware/build details are still missing. Please do not present this as a completed assignment.

## Deliverables

- [candidate_canary.patch](candidate_canary.patch) — proposed C++ changes for Features A and B.
- [args.gn](args.gn) — GN build configuration.
- [ai_transcripts/](ai_transcripts/) — directory for genuine exported AI session logs (not yet included).
- This README — instructions and disclosures.

## Feature A: Canary HTTP header

Launch Chromium with `--inject-candidate-id=Shubham-Padkonde`. The patch attempts to forward the switch to the out-of-process Network Service and add `X-Candidate-Canary: Shubham-Padkonde` to HTTP/HTTPS requests using the URLLoader path. This has **not** been compiled or tested, and broader coverage (redirects, preflights, or other request paths) is unverified.

## Feature B: C++ console acknowledgment

The Blink console warning callback checks for `console.warn("canary-ping")` and attempts to emit a C++ log prefixed `[CANARY_ACK]` with the GitHub username and a runtime monotonic timestamp. The existing DevTools console behavior is intended to remain unchanged. Compiler and runtime behavior are unverified.

## Build steps (Linux example)

Use a real Chromium `src` checkout with its dependencies installed. First verify the patch applies against **your specific Chromium revision**; adapt the patch if upstream APIs have changed.

```bash
cd /path/to/chromium/src
git rev-parse HEAD
git apply --check /path/to/repo/candidate_canary.patch
git apply /path/to/repo/candidate_canary.patch
mkdir -p out/Default
cp /path/to/repo/args.gn out/Default/args.gn
gn gen out/Default
autoninja -C out/Default chrome
./out/Default/chrome --user-data-dir=/tmp/canary-profile \
  --inject-candidate-id=Shubham-Padkonde --enable-logging=stderr
```

Check requests with `https://httpbin.org/headers` and then evaluate `console.warn("canary-ping")` in DevTools. Negative tests should include no flag, wrong console message, different console level, HTTP and HTTPS, redirects, and subresources. Preserve actual command output.

## Assignment evidence — REQUIRED before submission

| Item | Status |
| --- | --- |
| Chromium checkout commit (`git rev-parse HEAD`) | **PENDING** |
| Machine OS and RAM | **PENDING** |
| Successful compilation, header check and console log | **PENDING** |
| Unedited ~5-minute Loom walkthrough URL | **PENDING** |
| Live marker change to `[CANARY_ACK_LIVE]` and actual rebuild on video | **PENDING** |
| Authentic AI interaction transcripts | **PENDING** |

## AI use disclosure

ChatGPT (GPT-6) assisted with generating a proposed patch and project documents. No other agent execution, tested build, successful runtime result, or recorded video is claimed. Place real transcripts of AI assistance in `ai_transcripts/`; do not invent or backfill tool sessions.

## Notes

- The patch uses a monotonic timestamp (microseconds from process/clock origin), **not** a wall-clock date/time.
- Injecting this header exposes the supplied identifier to HTTP(S) servers; use only a nonsensitive candidate ID.
- If the patch fails against current Chromium APIs, fix it in the real checkout and regenerate the final patch before submission.
