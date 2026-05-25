# Veil QUIC Patch Slots

This directory is reserved for reviewable QUIC/H3 native patch files.

Current stage:

- No active patch file is applied.
- The vendored quiche BoringSSL baseline remains at
  `f1c75347daa2ea81a941e953f2263e0a4d970c8d`.

Expected future patch slots:

- QUIC-TLS signature algorithm override for H3-specific profiles.
- QUIC-TLS extension order override for H3-specific profiles.
- Optional baseline-unification patch if quiche moves to the same BoringSSL
  family as `veil-boringssl`.

Any future patch must be small enough to review against the recorded baseline
and must be paired with a Rust API/wrapper verification note.

