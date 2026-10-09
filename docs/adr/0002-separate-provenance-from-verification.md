# Separate port provenance from compatibility verification

Record the upstream version and source snapshot from which each ported implementation was derived separately from the baselines against which its behavior has been verified. Preserve historical provenance when upstream advances, and identify affected implementations for review rather than treating their original porting version as proof of compatibility with a newer release. This costs an additional verification history but prevents progress tracking from silently overstating compatibility.

Each baseline pins its runtime dependencies as well as the main library. Identify repository sources by exact commit and dependency packages by exact version and archive integrity, because dependency changes can alter behavior without changing parser function bodies.

A port unit counts as ported only when its Crystal implementation exists, its upstream correspondence is recorded, and relevant compatibility tests pass. Incomplete implementations or evidence remain partial; internal functions may be exercised through public API tests without matching the upstream function boundaries.

Changes to upstream functions, surrounding modules, relevant dependencies, or mapped Crystal code conservatively flag affected units for review. Historical provenance remains, but current verification requires the relevant tests to be rerun and the change reviewed; unchanged function hashes alone do not verify compatibility with a newer baseline.

Passing evidence identifies the exact upstream runtime dependency closure, Crystal sources, tests, fixtures, comparison harness, and toolchain used. Initially, changes anywhere in this verification context make current evidence stale until the relevant checks and review are renewed; this deliberately favors conservative whole-project invalidation over an unproven dependency analysis. Comparisons preserve types, signed zero, metadata, errors, and callback behavior rather than losing them through ordinary JSON serialization.
