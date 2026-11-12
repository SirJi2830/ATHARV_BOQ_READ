# VBA-dependent BOQ processing plan

## What the current app can do
The current reader uses Apache POI to open `.xls`, `.xlsx` and `.xlsm` files and read worksheet cells and formula results. It does not run VBA.

## Why a generic “run every macro on Android” feature is not included
- Android does not ship with Microsoft Excel's VBA runtime.
- Apache POI does not execute VBA projects.
- Macro-generated rows, hidden sheets, or values may not exist in the workbook until the workbook's own code runs.
- Running arbitrary macros from unknown files can be unsafe.

## Recommended path for a particular PWD BOQ
1. Obtain a copy of the original workbook and, if authorized, its VBA source/project.
2. Identify the workbook's entry macro, the sheets/ranges it reads, and the exact output sheet/range it creates.
3. Create test copies and record expected output from a trusted desktop Excel environment.
4. Prefer implementing the extraction/calculation logic directly in Kotlin when feasible.
5. If Excel-specific execution is unavoidable, design a separate authenticated processing service using a trusted licensed environment. Validate uploads, isolate processing, limit file size/time, scan files, and never execute untrusted macros automatically.
6. Compare the Android/service output against known-good Excel output before using it for tender costing or submissions.

## Extension point
Keep workbook import behind `BoqWorkbookReader`. A future implementation can add a `MacroDependentBoqProcessor` interface and route only known, explicitly supported workbook templates through their own parser/calculation logic.
