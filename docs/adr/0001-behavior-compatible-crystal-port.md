# Preserve upstream behavior through an idiomatic Crystal API

The intended port covers the parser, validator, and builder, including the dependency behavior they rely on, and will be delivered incrementally. Preserve upstream observable behavior for parsed values, options, validation results, and XML output while allowing idiomatic Crystal APIs and different internal function boundaries; function correspondence will be explicit rather than requiring a one-to-one translation. This favors a usable Crystal library while retaining a concrete compatibility contract with fast-xml-parser.

The initial upstream baseline is fast-xml-parser v5.11.2 at commit `80e88a848b073fd630bc181291a24922c6d1e839`. Both release version and exact commit identify the source; `XMLBuilder` is supplied by the separate fast-xml-builder package, so its implementation requires its own upstream reference.

Inventory individual functions, methods, and callbacks in the v5 library implementation, including internal helpers and dependency functions on which the port relies. Experimental v6 code, CLI tools, and generated bundles are outside the port inventory.

When upstream documentation or declarations disagree with executable behavior at the pinned baseline, executable behavior is the compatibility reference. Record discrepancies and deliberate deviations explicitly so neither silently changes the compatibility contract.

Support the same features as the upstream library, including its observable edge cases. This includes numeric conversion and signed zero, UTF-16 position units with upstream newline normalization, BOM and malformed-byte handling, permissive parsing when validation is disabled, reserved-name handling, builder property ordering, and metadata behavior. Crystal-specific representations must preserve the corresponding behavior rather than silently substituting native arithmetic, offsets, or validation rules.

Preserve both compact and order-preserving output and upstream callback expressiveness, including scalar and container results, callback-supplied objects represented by native Crystal counterparts, callback invocation and mutation behavior, and the distinction between replacement and keeping original text. An initial restriction to JSON-compatible callback values would narrow the accepted contract and cannot be treated as full compatibility.
