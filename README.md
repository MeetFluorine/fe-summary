# Issuance & Engineer Allocation – Auto Dashboard

A single-file HTML tool that turns the raw **Report_Issuance_Recover_PAN_India** download into:

1. A store-wise transaction count (Sheet4).
2. An engineer-wise item allocation pivot with red / green checks.

**Live site:** http://meetfluorine.github.io/fe-summary

Everything runs in your browser. The raw file is never uploaded anywhere. The Excel library is built into the page, so it works without internet.

## How to use

1. Open the live site (or `Issuance_Engineer_Dashboard.html` locally) in Chrome or Edge.
2. Drop the raw `.zip` or `.xlsx` into the box. A progress bar shows the stages.
3. Check the counts and the tabs.
4. Click **Download Excel**. The file is saved as `<raw file name>_Processed.xlsx`.

## What it does

| Step | Rule |
|---|---|
| 1. Filter | Keeps rows where `Substore_Locator` contains `SDFX` |
| 2. Map | Looks up the locator in the mapping table to add **SS** (SFX Store Name) and **Circle** |
| 3. Sheet1 | Removes duplicate `Transaction Number` rows, keeping the first |
| 4. Sheet4 | Counts transactions by SS and Circle |
| 5. Engineer detail | Uses all SDFX rows (no duplicate removal). `Issued_Qty` becomes 1 when `Serial_Number` is not blank |
| 6. Rename (with serial number) | `iptv` becomes Iptv, `ONT` becomes Ont, `wifi router` becomes Wifi Router, `modem outdoor` becomes W- Modem Outdoor |
| 7. Rename (no serial number) | By item code: B0HLANJT4 becomes Cat 6, B0IMPXV5 becomes Face Plate |

## Tabs

- **Store counts (Sheet4):** SS, Circle and count of Transaction Number, with a Grand Total.
- **Engineer pivot:** filters Circle and Txn_Type, rows SS then Engineer_Name, columns Cat 6, Drop Fiber accessories, Face Plate, Ont, W- Modem Outdoor, Wifi Router and Grand Total (sum of Issued_Qty).
- **Sheet1 – transactions** and **Engineer detail:** previews of the cleaned data.

## Red / green checks

The checks run when **Txn_Type = Allocate**. Recover rows are not issuances, so they are not coloured.

A row is **red** when any of these is exceeded, otherwise **green**:

| Check | Limit |
|---|---|
| Cat 6 | 20 m per set. A set is one W- Modem Outdoor plus one Wifi Router (the smaller of the two counts) |
| Face Plate | 1 per Ont |
| Drop Fiber accessories | 1 per Ont |

Hover a red row to see the reason. The limits can be changed under **Pivot checks** in the left panel.

## Excel download

| Sheet | Contents |
|---|---|
| Sheet1 | Unique transactions with SS and Circle |
| Sheet4 | Store counts |
| Engineer Detail | All SDFX engineer rows, renamed, with SS and Circle |
| Engineer Pivot | All circles and both Txn_Types, with filter dropdowns and red / green fills on Allocate rows. A "Total of visible rows" line updates when you filter |
| Sheet2 | The locator mapping used |

## Editing the rules and mapping

All of these are on the left side and are remembered in your browser. Click **Apply rules** to refresh.

- **Serialized rules:** `pattern => New name`, first match wins. The pattern is a case-insensitive regex.
- **Non-serialized rules:** `item_code => New name`.
- **Locator mapping:** edit any cell, add a row, delete a row, search, or paste rows from Excel (4 columns). A locator not found in the table is flagged in red, with a button to add it to the mapping.

The mapping is saved per browser. On another computer or browser, paste your updated table in again.

## Troubleshooting

| Problem | Fix |
|---|---|
| Nothing happens after dropping the file | Reload with Ctrl+F5 and drop the file again |
| "Could not read the file" | Use the original `.zip` or `.xlsx` from the download, not a re-saved copy |
| "Unmapped locators" is above 0 | Click the red button to add them, fill in Circle and SFX Store Name, then click Apply rules |
| No red / green colours on the page | Set Txn_Type to Allocate |
| A description was not renamed | Add or adjust the line in the rules box, then click Apply rules |

## Files

- `Issuance_Engineer_Dashboard.html`: the tool (one file, no install).
- `README_Issuance_Engineer.md`: this guide.
