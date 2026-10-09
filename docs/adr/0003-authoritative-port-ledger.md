# Track function correspondence in an authoritative port ledger

Keep a version-controlled YAML port ledger containing mappings, provenance, implementation status, and compatibility evidence, and generate a readable progress report from it. This favors machine-checkable records over separate hand-maintained progress lists, which can disagree as the port grows.

Each upstream port unit has a stable ledger ID. Its locations at particular baselines are recorded separately using package instance, source path, and qualified function name; renames and moves require explicit reconciliation because names collide and line numbers move.

Mappings may associate several upstream functions with one Crystal method or one upstream function with several Crystal methods, including equivalent standard-library operations. Each upstream unit still requires identifiable behavioral evidence, and a missing implementation cannot disappear merely because the Crystal code uses different boundaries.

A JavaScript syntax-tree scanner discovers every function, method, and library-defined callback within the reviewed runtime scope; new or ambiguous entries require explicit classification. Constants, imports, entity tables, and other data are also tracked as behavioral context so function-body hashes cannot conceal behavior changes.

Reports distinguish historical porting from current verification, retain an explicit inventory total for each baseline, and expose pending, partial, and stale work. CI rejects missing inventory entries and unsupported verification claims while permitting unfinished porting work, so the initial report can truthfully show zero ported units.
