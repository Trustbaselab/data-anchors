# Data Anchors

**Public, verifiable proof that specific data artifacts existed at specific times.**

---

## What this repository is

A **timestamp ledger** for Trustbase Lab's data assets.

Each entry records:

- the **SHA-256 hash** of a data file
- the **version** it belonged to
- the **date** it was anchored

That is all. Nothing about the contents, the structure, or the sources is
recorded here.

## Why publish hashes and nothing else

Because verification and disclosure are different things, and conflating them is
the most common mistake in data infrastructure.

**Verification requires one thing:** the ability for anyone to check that the
bytes you have today are the same bytes that existed on a given date. A hash does
that completely. It reveals nothing — a SHA-256 digest is a one-way fingerprint.
Given the hash, you cannot reconstruct the file, learn its size, or infer its
contents.

**Disclosure is a separate decision.** The structure of our dataset — how many
modules, how many records per module, which sources — is our work product. It
does not need to be public for the data to be verifiable.

So this repository publishes exactly one thing per release: **the hash, the
version, and the date.**

> **Verifiable, not replicable.**

This is not a limitation of the format. It is the format's entire point.

---

## How verification works

Anyone can verify that a copy of our data matches an anchored version:

```bash
# 1. Compute the hash of the file you have
sha256sum data.json

# 2. Compare with the value in anchors/<version>.txt
#    If the two match, the file is byte-identical to the anchored version.

# 3. Confirm the anchor's own timestamp
git log --format='%H  %ci  %s' -- anchors/
```

If the hashes match, the file has not changed since it was anchored.
If they differ, either you have a different version — check for a newer anchor —
or the file has been modified.

**No account, no permission, no intermediary, no fee.**

---

## What this gives us

A chain of evidence with four independent layers:

| Layer | What it proves | Who holds it | Cost |
|---|---|---|---|
| **SHA-256 hash** | The exact content | Algorithm itself | 0 |
| **Git commit** | When this hash was submitted | GitHub (third party) | 0 |
| **Daily server backup** | The data at each day's end | Our own server logs | 0 |
| **Printed + registered mail** | Official postal timestamp | 中国邮政 (China Post) | ~5 CNY |

None of these requires trusting a vendor. None of them requires a subscription.
None of them expires.

Every layer can be checked independently. Four layers means no single point of
failure, and no single party whose honesty must be assumed.

---

## What this is *not*

- **Not a timestamp authority.** We do not issue certificates.
- **Not a notary.** We do not attest to content, legality, or ownership.
- **Not proof that our data is correct.** A hash proves integrity, not truth. A
  file full of errors has a perfectly valid hash.
- **Not immutable.** We control this repository. We could, in principle, rewrite
  history — but that would break every external copy, every backup log, and every
  printed record. The four-layer design exists precisely so that no one has to
  take our word for it.

---

## A note on Bitcoin

An earlier draft of this design proposed anchoring hashes to the Bitcoin
blockchain — free, cryptographically the strongest option available, and widely
used internationally for exactly this purpose.

**We did not do it.**

The technical case was solid. The regulatory case was not. Under 银发〔2021〕237号
(2021-09-15), virtual-currency *financial activities* are prohibited in mainland
China. Writing a hash into a blockchain is not one of those activities — it
involves no currency, no exchange, no trading — but the explanation cost inside a
government registration process would be high, and this is not a company that can
afford ambiguity about its own compliance.

**Technical optimality is not the same as fitness for this position.** We chose
the boring path.

---

## Format

```
anchors/
  2026-10-28-v0.20.txt
  2026-10-28-v0.19.txt
  ...
```

Each file is three lines plus a comment block:

```
version: v0.20
date: 2026-10-28
sha256: b687a7023a7acc741cb57b9fe40b7748827a159e3993e013ee77a43b473b76e5
```

---

## License

The anchor records in this repository are released into the public domain
(CC0 1.0). They are facts, not creative works.

---

*Trustbase Lab · 山西至信恒基科技有限公司 · 2026*
