# Crystal port with function provenance and compatibility verification

Closed: no
Design confirmation: pending final shared-understanding review

## Outcome

Port the fast-xml-parser library to Crystal with the same supported features and observable behavior as the upstream baseline. Provide a reliable answer to which upstream functions have been ported, which source versions they came from, and which upstream and Crystal revisions have passing compatibility evidence.

The agreed decisions are recorded in the root [glossary](../../GLOSSARY.md) and [ADRs](../../docs/adr/). This spec makes those decisions concrete for implementation; it does not claim that the scaffold already implements them.

## Baseline and scope

- Initial repository baseline: fast-xml-parser **v5.11.2**, commit `80e88a848b073fd630bc181291a24922c6d1e839`, retained through the `fast-xml-parser-js` reference submodule.
- Required public components: parser, validator, and builder, including the runtime dependency behavior they rely on.
- Inventory functions, methods, constructors, and library-defined callbacks in the reviewed v5 runtime source scope, including internal helpers. User-supplied and test-defined callbacks are behavioral inputs, not additional upstream port units.
- Exclude experimental v6, CLI tools, development/test dependencies, and generated duplicate bundles from the required function inventory. Record the source-scope rules so exclusions can be reviewed and reproduced.
- Pin the complete resolved runtime dependency closure. For packages, record version, archive integrity, and dependency-instance identity; for repository sources, record exact commit. Retain package manifests and dependency resolution as behavioral context.

The checked-in lockfile resolves the direct runtime dependencies to `@nodable/entities 3.1.0`, `fast-xml-builder 1.2.0`, `is-unsafe 2.0.0`, `path-expression-matcher 1.6.2`, `strnum 2.4.2`, and `xml-naming 0.3.0`. It also contains transitive `anynum 1.0.1` and a separate builder dependency instance of `xml-naming 0.1.0`. Materialize and integrity-check the complete runtime closure from the baseline lockfile; this list is explanatory, not a substitute for dependency resolution.

`src/xmlbuilder/json2xml.js` re-exports fast-xml-builder, so the builder implementation has separate package provenance. Generated bundles or source maps can assist investigation, but do not replace identified original sources in the port inventory.

## Compatibility contract

Use executable behavior at the pinned baseline as the reference when upstream documentation or declarations disagree. Preserve all supported options and their defaults, compact and order-preserving output, entities and their limits, validation results, builder output, metadata, and callback behavior. Record documentation discrepancies and deliberate deviations explicitly; an unimplemented feature remains unfinished work rather than a waived requirement.

The contract includes numeric coercion and signed zero; UTF-16 position units and upstream newline normalization; BOM and malformed-byte handling; permissive parsing when validation is disabled; reserved-name rejection/sanitization; and JavaScript property-order effects on builder output.

Crystal APIs and internal boundaries may be idiomatic. Their value representation must retain scalar/container distinctions, ordered output, metadata, and callback-supplied native object counterparts. Preserve callback arguments, invocation order, mutations, and the distinction between replacing a value and keeping original text. Restricting callback results to JSON-compatible values does not satisfy the full contract.

## Port ledger

Use a tracked `porting/` directory for the YAML port ledger and its generated artifacts. The location avoids the current ignore rule for most of `docs/`; paths and command names are implementation details, while the ledger remains the authoritative record.

The ledger records:

| Record | Required information |
| --- | --- |
| Upstream baseline | Release, exact repository commit, runtime package instances and integrity, reviewed runtime source scope |
| Port unit | Stable ID and per-baseline package-instance, source-path, and qualified-symbol locators |
| Port mapping | Upstream unit IDs, Crystal implementations or equivalent standard-library operations, and the behaviors the mapping preserves |
| Implementation state | Pending or partial work and the recorded revisions at which a unit satisfied the ported criteria |
| Port provenance | The upstream source snapshots from which the implementation was derived, retaining prior provenance as later changes are adopted |
| Compatibility evidence | Supported unit IDs and behavior cases, passing results, exact verification context, and completed mapping/change review |

Function locations and hashes are separate from stable IDs. Line numbers are navigation hints. Renames, moves, and ambiguous identities require explicit reconciliation, and source splits or merges retain recorded lineage rather than overwriting historical identity.

Allow many-to-many mappings. For example, `XMLParser.parse` can correspond to several Crystal methods, while multiple upstream helpers can correspond to one Crystal operation. Each upstream unit still needs identifiable behavioral evidence; linking it to a passing feature test is not, by itself, proof that all its relevant behavior was exercised.

A genuinely JavaScript-runtime-only unit may retain an explicit reviewed disposition and rationale without a fabricated Crystal mapping. Keep it inventoried and report it separately without counting it as ported; any required observable behavior still needs a Crystal counterpart and evidence. This classification cannot waive the feature-support contract.

Illustrative pending entry, before any implementation exists:

```yaml
id: fxp.xmlparser.parse
upstream_locations:
  - baseline: fxp-5.11.2
    package_instance: fast-xml-parser-root
    path: src/xmlparser/XMLParser.js
    symbol: XMLParser.parse
implementation_status: pending
implementations: []
ported_from: []
verification_history: []
```

The final schema may refine these field names. It must preserve these distinctions and validate references mechanically.

## Discovery and drift

Use a JavaScript syntax-tree scanner to discover every function, method, and library-defined callback in the explicitly reviewed runtime scope. Cover named functions, methods, anonymous option callbacks, assigned functions, nested functions, and returned closures. Discovery must expose new, removed, and ambiguous candidates for explicit classification rather than silently assigning historical identities.

Track whole participating source files, constants, imports, entity tables, and other relevant data as behavioral context. Inventory snapshots are retained per baseline so additions and removals change that baseline's totals explicitly.

Source and dependency changes produce a drift report for review. They do not rewrite port provenance or automatically verify compatibility. A removed or changed Crystal implementation must remain visible even if a prior revision was successfully ported.

Initially, verification freshness uses conservative whole-project invalidation across the exact upstream source/dependency closure, Crystal source, tests, fixtures, harness, and toolchain. Retain historical passing records when inputs change, mark current evidence stale, and renew it only after the relevant checks and change/mapping review are complete. Changes to evidence records or generated reports must not create a self-invalidating fingerprint cycle; changes to mappings and executable verification inputs must be accounted for.

## Comparison harness and evidence

Execute shared behavior cases against the pinned JavaScript reference and the Crystal implementation. Use a tagged comparison representation that preserves scalar types, signed zero, nonfinite numbers, order, metadata, error details and positions, and callback traces. Custom object cases need explicit counterparts and observations; ordinary JSON serialization cannot silently erase unsupported values or identity-sensitive behavior.

Include relevant upstream fixtures and regression cases. Internal units may be exercised through the public API, with explicit links from evidence to the units and behaviors covered. Tooling checks evidence references and execution results; sufficiency of behavioral coverage and mapping correspondence also requires review.

Evidence binds the baseline and runtime package instances to the exact Crystal source, test/fixture/harness snapshots, runtime/compiler versions, and any resolved development-tool dependencies used. Include both successful comparisons and the verification command/result that produced them. A changed source hash can invalidate evidence; an unchanged hash cannot establish compatibility on its own.

A port unit is historically ported only after implementation, recorded correspondence, and passing relevant compatibility evidence exist at an identified Crystal revision. Expired evidence makes current verification stale without erasing that historical result. An incomplete implementation or incomplete behavioral evidence is partial.

## Report and CI

Generate the readable progress report from the ledger, inventory, and verification results. For each baseline, show the inventory total and separate counts for pending work, partial implementation, historically ported units, currently verified units, and stale evidence. These counts may overlap across the implementation and freshness dimensions; do not add them as if they were one partition. Show component-level progress and unit-level source provenance and verification references.

Preserve explicit scope/exclusion reasons and removed units in baseline history. Function counts describe porting progress, not proof that the entire public feature contract is complete.

CI checks schema and reference validity, complete discovery/classification, source and implementation correspondence, deterministic report generation, and the evidence needed for verification claims. Detect stale inputs rather than accepting a manually asserted verified flag. Unfinished units remain visible and do not fail CI simply because the whole port is not yet complete. Current verified claims require matching evidence and passing relevant checks.

## Delivery sequence and exit criteria

1. **Baseline and discovery:** capture the immutable upstream/runtime baseline and reviewed source scope; generate a complete initial inventory with explicit classifications and zero claimed ports.
2. **Ledger, drift, and report:** validate the ledger, preserve identity/provenance history, expose additions/removals/ambiguities and stale contexts, generate the readable report, and add CI checks that allow truthful unfinished work.
3. **Comparison harness:** run shared cases against the pinned reference and Crystal, verify the tagged protocol's handling of values/metadata/errors/callbacks, and bind evidence to exact inputs. Future features may remain pending, but the harness cannot report lost comparison information as a pass.
4. **Validator slice:** port a small coherent validator capability and demonstrate discovery, mapping, provenance, passing comparisons, report updates, and intentional staleness after an input change. Keep the rest of the validator explicitly unfinished.
5. **Complete the library:** finish the validator, then parser, then builder, bringing their internal helpers and required dependency behavior through the same workflow. Full feature-support claims require completion of the accepted compatibility contract as well as complete function bookkeeping.

Crystal is the library runtime. Node.js may support development-time source discovery and the upstream comparison oracle. Pin development-tool dependencies used for reproducible verification; runtime users of the Crystal library should not need the JavaScript oracle.

## Acceptance criteria

- A report identifies every in-scope upstream port unit, its implementation state, its port provenance when implemented, and the baselines/revisions covered by compatibility evidence.
- Discovery cannot omit an unclassified function silently, including anonymous and nested callbacks within the reviewed runtime scope.
- Dependency changes, surrounding source/data changes, and Crystal/test/harness/toolchain changes invalidate current evidence as specified while preserving history.
- Renames, moves, splits, merges, and removals can be reconciled without deleting prior provenance or verification records.
- The first validator slice demonstrates the entire tracking and verification workflow with meaningful passing comparisons and an observed stale-input transition.
- The eventual Crystal library supports the same upstream features and observable behavior; partial work and deviations cannot masquerade as full compatibility.

## Comments

- 2026-10-09: The design interview accepted Q1-Q12, reaffirmed full upstream support in response to Q13-Q16, and accepted Q17-Q20. Final shared-understanding review is pending before implementation, as required by the invoked grilling workflow.
