# Fast XML Parser Crystal Port

This project ports fast-xml-parser behavior to Crystal and tracks its relationship to upstream releases.

## Language

**Upstream baseline**:
An identified snapshot of the upstream library against which the Crystal port is developed or assessed.
_Avoid_: Latest upstream, target version without an identified snapshot

**Port provenance**:
The upstream source snapshot from which a Crystal implementation was derived. Provenance remains historical even when compatibility is assessed against a newer baseline.
_Avoid_: Supported version, current compatibility

**Compatibility verification**:
Evidence that a Crystal implementation preserves the agreed upstream observable behavior at a particular baseline.
_Avoid_: Port provenance, version copied from

**Upstream drift**:
A change since a recorded upstream baseline that may affect the Crystal port's behavior or correspondence to upstream source.
_Avoid_: Automatically incompatible, automatically compatible

**Port unit**:
An individual upstream function, method, or callback whose implementation and compatibility are tracked in the Crystal port.
_Avoid_: Feature, source file

**Ported**:
A port unit with a corresponding Crystal implementation and passing compatibility evidence at a recorded Crystal revision against an identified upstream baseline. This historical result does not establish compatibility with later upstream or Crystal revisions.
_Avoid_: Code written, currently compatible

**Partial**:
A port unit with an incomplete implementation or incomplete compatibility evidence. Expired evidence alone affects verification freshness rather than making a historical port partial.
_Avoid_: Ported

**Port ledger**:
The authoritative record of port units, their correspondence to Crystal implementations, their provenance, and their compatibility evidence.
_Avoid_: Progress report, checklist

**Port mapping**:
A recorded correspondence between one or more upstream port units and one or more Crystal implementations or equivalent standard-library operations.
_Avoid_: One-to-one translation

**Verification freshness**:
Whether recorded compatibility evidence still applies to the current upstream and Crystal implementation context.
_Avoid_: Port provenance, implementation status
