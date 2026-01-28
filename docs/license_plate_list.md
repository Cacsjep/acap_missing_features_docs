# License Plate List

Manage a whitelist of license plates (with validity windows and tags) and expose it to Flow and other features.

---

## Overview
- Store plates with optional tags, names, and validity windows.
- Optional fuzzy matching (max deviation 1–2 characters) for near-miss reads.
- Automatic cleanup of expired plates (configurable interval).
- Plates are cached in-memory and served to the Flow **License Plates** node.

---

## Enabling the feature
1) Open **License Plate List** in the UI and enable the feature.
2) (Optional) Set **Allow Deviation** and **Max Deviation** (1–2) to permit slight mismatches.
3) (Optional) Set **Cleanup Interval (minutes)** to auto-remove expired entries (0 = off).

---

## Adding plates
- **Plate**: required, stored uppercased.
- **Name**: optional label (owner/vehicle).
- **Tags**: optional labels (e.g., `staff`, `vip`, `visitor`); duplicates and spacing are normalized.
- **Valid From / Valid To**: optional; truncated to the hour; omitted means always valid.

Save to commit changes; the cache refreshes immediately.

---

## Flow integration
Use the **License Plates** node in Flow:
- Input: plate string, Process (rising edge), Enter, Exit, Reset.
- Outputs: matched plate, in-list/not-in-list, expired/not-yet-valid, total plates, tags string, label, status, enter, exit, per-tag booleans.
- Respects **Allow Deviation / Max Deviation** and validity dates from this feature.

!!! note "Tag outputs and node updates"
    When you add or remove **tags** in the License Plate List feature, existing License Plate nodes in your flows will **not** automatically gain or lose tag outputs. The plate data itself updates immediately, but the node's output structure is set when the node is created.

    To use newly added tags as boolean outputs, you must **delete and recreate** the License Plate node in your flow. This design prevents disrupting running flows when plate entries are modified.

---

## Cleanup behavior
When **Cleanup Interval** > 0:
- Periodically removes plates whose **Valid To** is in the past.
- Persists the pruned list and refreshes the cache.

---

## Notes
- If the feature is disabled, Flow will not receive plate data.
- Validity times are parsed in common formats (ISO 8601 recommended).
- Max deviation is clamped between 1 and 2 characters.
