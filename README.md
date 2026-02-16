# Menu Data Proofs

This repository contains SHA-256 hashes of a menu intelligence dataset, committed monthly to provide verifiable proof of data existence at each point in time.

## How It Works

Each month, the full dataset is exported as deterministic JSON (sorted keys, consistent serialization) and hashed with SHA-256. Only the hash and aggregate metadata are committed here — **no actual restaurant, menu, or allergen data is published**.

The git commit timestamp provides an immutable record that this data existed on or before the commit date.

## Proof Format

Each proof file (`proofs/YYYY-MM.json`) contains:

```json
{
  "version": "1.0",
  "period": "2026-02",
  "generatedAt": "2026-02-15T12:00:00.000Z",
  "hash": {
    "algorithm": "SHA-256",
    "value": "a1b2c3d4..."
  },
  "metadata": {
    "restaurants": 150,
    "menuSnapshots": 420,
    "dishes": 8500,
    "beverages": 2100,
    "allergenVerifications": 12000,
    "dishVariantGroups": 3200
  },
  "filter": {
    "type": "cumulative",
    "cutoffDate": "2026-03-01T00:00:00.000Z"
  },
  "note": null
}
```

- **hash**: SHA-256 of the complete deterministic JSON export
- **metadata**: Aggregate record counts (no actual data)
- **filter**: `cumulative` means all data up to the cutoff date
- **note**: Present on retroactive proofs to indicate they were generated after the data period

## Verification

If you have access to the dataset export for a given period, verify it matches the proof:

```bash
# macOS
shasum -a 256 export.json

# Linux
sha256sum export.json
```

The resulting hash should match the `hash.value` in the corresponding proof file.

The export must use identical deterministic JSON serialization (sorted keys, ISO date strings) to produce the same hash.

## Retroactive Proofs

Proofs for periods before this repository was created are marked with a `note` field explaining they were generated retroactively. The data itself existed at the stated time (verified by server-generated database timestamps), but the hash commitment was made after the fact.

Starting with the month this repository was created, all proofs are generated contemporaneously.

## What This Does NOT Contain

- No restaurant names, addresses, or URLs
- No menu items, prices, or descriptions
- No allergen data or ingredient lists
- No API keys or credentials
