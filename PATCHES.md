# Patch Pipeline

`veil-quic` carries the QUIC/H3 side of the fingerprint surface. It currently
uses quiche's vendored BoringSSL baseline, which is not the same BoringSSL
commit as `veil-boringssl` / `veil-tls`.

## Baseline

- BoringSSL submodule: `quiche/deps/boringssl/src`
- Current commit: `f1c75347daa2ea81a941e953f2263e0a4d970c8d`
- TLS baseline used by `veil-boringssl` / `veil-tls`:
  `673e61fc215b178a90c0e67858bbf162c8158993`

This baseline divergence is intentional for this stage: document and verify the
gap before changing the quiche dependency. Do not silently force the BoringSSL
commit to match `veil-boringssl` without a separate upgrade plan.

## Patch Set

No root patch file is active yet for the H3-specific QUIC-TLS overrides.
`patches/README.md` records the expected future slots.

Native behavior changes for QUIC-TLS must become patch files before they are
exposed through Rust APIs. This keeps curl-impersonate parity review separate
from runtime integration.

## Current Surface

Supported by existing quiche/tokio-quiche APIs:

- QUIC transport parameters such as initial flow-control windows, stream limits,
  active connection ID limit, idle timeout, ACK delay exponent, max ACK delay,
  GREASE, and unknown transport parameter tracking.
- H3 settings through `quiche::h3::Config` and tokio-quiche settings:
  max field section size, QPACK table capacity, QPACK blocked streams, and
  additional settings.
- H3 pseudo-header order can be constructed by the upper layer because quiche
  accepts ordered header vectors.

Known QUIC-TLS gaps against curl-impersonate:

- H3-only signature algorithm override.
- H3-only TLS extension order override.
- The full TLS fingerprint patch set from `veil-boringssl` is not yet unified
  into quiche's vendored BoringSSL baseline.

## Verification Contract

1. Verify the current quiche BoringSSL baseline commit explicitly.
2. Verify H3 settings and QUIC transport parameter APIs before adding new
   patches.
3. Mark missing QUIC-TLS symbols as expected gaps until a patch file lands.
4. Run the native Cargo check from a VS2022/MSVC shell on Windows.

