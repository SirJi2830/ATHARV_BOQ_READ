# Atharv BOQ Reader — GitHub-ready Android project

This project is intended to let you build a test APK using GitHub Actions from a phone. The app is an offline-first spreadsheet BOQ viewer.

## Included in this starter version

- Imports Excel `.xls`, `.xlsx` and `.xlsm` workbooks through Android's document picker.
- Reads worksheet names and stored cell/formula values.
- Displays rows and values, with a basic scan for common BOQ headings such as Item, Description, Unit, Quantity, Rate and Amount.
- Shows a warning when a sheet contains the text “Please Enable Macros to View BoQ information”.
- Exports the currently selected worksheet to CSV.
- Can select files from local storage or cloud-storage providers shown by Android's document picker (for example, a connected Drive provider). The workbook is processed on the phone after it is opened.

## Important: VBA macro limitation

**This APK does not execute VBA macros.** Apache POI reads workbook content and can evaluate many ordinary spreadsheet formulas, but it is not Microsoft Excel and does not run embedded VBA code. If a BOQ workbook creates or reveals its real table only after a macro runs, this version may show the macro warning or only the stored/visible workbook content; it cannot promise to reconstruct the generated BOQ.

A safe next step for a macro-dependent BOQ is to inspect that specific workbook and its VBA source, then either:
1. implement the required workbook-specific calculations and data extraction in Kotlin; or
2. use a trusted, separately hosted Windows/Excel-compatible processing service, with explicit security controls and user consent.

Do not automatically run macros from unknown or untrusted workbooks. LibreOffice compatibility with Excel VBA is incomplete and must be tested against the exact workbook.

## Build the APK using an Android phone

1. Download and extract this ZIP on your phone.
2. Open [GitHub](https://github.com/) in your browser and sign in.
3. Create a new repository named `Atharv-BOQ-Reader`.
4. Upload the **contents** of this extracted folder to the repository root. Keep the hidden `.github/workflows/build-apk.yml` file and its folders; GitHub Actions needs that exact path.
   - If the mobile upload page does not let you upload folders, use GitHub's web editor or GitHub's mobile app/browser in desktop mode to create/upload the files while preserving their paths.
5. Open the repository's **Actions** tab.
6. Select **Build Atharv BOQ Reader APK** and tap **Run workflow**. A push to the `main` branch also starts a build.
7. Wait for the workflow to finish successfully. Open the completed run and find **Artifacts**.
8. Download `Atharv-BOQ-Reader-debug`, extract that downloaded artifact ZIP, then install `app-debug.apk`.
9. If Android blocks installation, allow “Install unknown apps” for the browser or file manager you used, then retry. Only install APKs you built and trust.

## Technical build details

- Android Gradle Plugin: 8.7.3
- Gradle: 8.9 (installed by the GitHub Actions workflow)
- JDK: 17
- Minimum Android: Android 8.0 / API 26
- Target SDK: 35
- Output: unsigned debug APK for testing, not a Play Store release APK.

## Current limitations / next development steps

- PDF viewing and conversion are not included in this starter version.
- BOQ heading detection is a basic heuristic; different department templates may need custom mapping.
- CSV export is available for the selected worksheet; styled Excel/PDF export is not included.
- The app has no dedicated backend. Cloud-file access depends on Android document providers; actual workbook parsing is local/offline.
- The workflow builds an APK, but this project has not been run on your phone or verified against your specific macro-enabled BOQ file yet.
