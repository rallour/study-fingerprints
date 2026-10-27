# Study fingerprints

A public, timestamped record for an ongoing statistical study. It contains
**no data**, only SHA-256 fingerprints (hashes) of:

1. **The pre-registered study plan** (`plan_versions.csv`): every version of
   the plan, which fixes the hypotheses, tests, significance level, sample size
   and stopping rule before the confirmatory data was collected. Later
   versions only add dated changelog entries.
2. **Every raw data file** (`fingerprints.csv`), hashed when it was saved.
   Files are listed under neutral labels (`capture_001`, ...). Some have a
   second, "clean" hash: a copy with credentials removed, which is the version
   that will be released.
3. **The analysis code** (`code_versions.csv`): one fingerprint per version
   of the code, covering every file in it. The code that produces the final
   results is fixed here before those results are computed.

## Why

A hash reveals nothing about a file's contents, but any change to the file,
even one character, produces a completely different hash. Each update to this
repository is also snapshotted by an independent archive (see
`timestamps.md`), so its contents at each point in time can be checked
without trusting the author's clock. Together they show that the plan and
each data file existed, in exactly this form, no later than the snapshot
date. That rules out choosing the tests after seeing the results, or editing
the data afterwards.

## Verifying (once the files are released)

```bash
sha256sum <file>    # compare with plan_versions.csv / fingerprints.csv
# analysis code, from the released code's top directory; compare with code_versions.csv
find src scripts -type f -not -path '*/__pycache__/*' | LC_ALL=C sort | xargs sha256sum | sha256sum
```

When the study is complete, the plan, the data files (with a table mapping
them to the labels used here), the analysis code and the results will be
published, whatever the outcome.
