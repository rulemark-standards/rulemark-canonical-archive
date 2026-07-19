# RuleMark Publishing Integrity Policy

Status: Normative operational policy  
Policy version: 1.0  
Applies to: Canonical Archive, Records, Machine Interface, RuleMark Web

## Purpose

Every public representation of a RuleMark standard must resolve to one canonical identity and one normative PDF artifact. A release must be rejected when the PDF, signed integrity data, registry metadata, machine record, human page, or download endpoint disagree.

The frozen PDF is normative. JSON supports discovery, citation, and verification; it must never silently replace or contradict the PDF.

## Required release chain

The release gate must validate this chain before publication:

```text
PDF content
  -> public filename and canonical ID
  -> SHA-256, byte length, and page count
  -> Canonical Archive registry and integrity baseline
  -> Records artifact
  -> Machine JSON
  -> Registry JSON
  -> human standard page and download endpoint
```

Any mismatch is a blocking failure.

## Required checks

For every standard, the gate must verify:

1. The standard has a unique canonical ID.
2. The PDF contains the required identity text declared in `integrity/standards.json`.
3. New standards use the canonical ID as the PDF filename. Any legacy public filename must be explicitly declared as an alias and must be byte-identical to the canonical-ID file.
4. SHA-256, byte length, and page count match the integrity baseline.
5. Any PDF copy held in the Canonical Archive is byte-identical to the Records artifact.
6. Any signature or checksum record matches the normative PDF.
7. Canonical Archive metadata agrees on ID, title, version, status, and canonical URL.
8. RuleMark Web source metadata agrees on ID, title, version, hash, bytes, URL, and citation.
9. Registry JSON, versioned Machine JSON, the human page, and the download response agree with the same baseline.
10. Publication date and registry issue date are not silently invented. A missing historical value remains `null` until supported by evidence.

## Frozen artifacts

A frozen PDF must not be edited or regenerated in place. A change to normative content requires a new version and a new hash.

A file-routing or publication-mapping correction is not a content amendment when the restored file is byte-identical to the previously frozen and signed artifact. In that case, the existing version remains unchanged and the signed historical hash controls.

## Legacy identity exceptions

Some initial standards predate canonical IDs inside the rendered PDF. They are declared as `title_only_legacy` in the integrity baseline. This is a compatibility exception, not a template for new releases.

All new standards must:

- display the canonical ID in the PDF;
- use `<canonical_id>.pdf` as the canonical filename;
- declare a version and publication date;
- include a signed SHA-256 record before publication.

## Release procedure

1. Add the candidate PDF and metadata without modifying an existing frozen artifact.
2. Update `integrity/standards.json` from verified evidence.
3. Run the local integrity gate from the Machine Interface repository.
4. Open a pull request and require the `RuleMark Integrity Gate` check.
5. Merge only when every local and online check passes.
6. After deployment, run the online gate again against the public URLs.

## Failure handling

Do not bypass or downgrade a failed check. Resolve the source of the discrepancy. Preserve evidence when a public artifact was incorrect. Never make multiple stores appear consistent by changing the signed baseline without documentary authority.

## AI-agent instruction

AI agents must treat the frozen PDF and its signed hash as the normative source. Agents may prepare metadata and release candidates, but must not modify frozen PDFs, mint unsupported dates, suppress failed checks, or publish when the integrity gate is red.

