# NSC File Explorer

Open a National Student Clearinghouse enrollment reporting file in your browser, read every student record as named, filterable columns, and find the records NSC would reject before you submit. You can also correct records and save the file back out.

**Use it now: <https://andersonzhu.github.io/nsc-file-explorer/>**

Everything runs in your browser tab. Files are read locally and never uploaded, so it is safe to use with student-level data. SSNs are masked on screen by default.

This is an independent tool built from the file specifications NSC publishes. It isn't made or supported by the National Student Clearinghouse.

## What it does

- **Reads both NSC file formats.** It opens fixed-width files, where every record is 1,250 characters, and tab-delimited text files saved from Excel. It recognizes the A3 (enrollment) or P3 (advanced registration) header, the D1 student records and the T1 trailer.
- **Splits every D1 record into its 115 fields.** Each field is labeled with its Excel column letter and character positions, for example `AW · 675–680` for Program 1 CIP Code.
- **Filters and sorts.** Type in the box under any column, or use the column's value list with counts. The box at the top searches the whole raw record, so it finds an SSN, ID or name anywhere.

  | Type | Meaning |
  |---|---|
  | `5138` | contains 5138 (codes also match their labels, so `withdrawn` finds enrollment status W) |
  | `=W` / `=H,L` | exactly W / exactly H or L |
  | `!=F` | anything except F |
  | `>20250825`, `20240101..20241231` | compare numbers and ranges (dates are YYYYMMDD, so this works for dates) |
  | `=` | blank |
- **Shows what codes mean.** Enrollment Status `H` shows as Half time, Credential Level `02` as Associate degree, and Special Program `N` as Not applicable. Program lengths stored as `002000` display as `2`.
- **Checks every record against NSC's rules.** It also validates the header, and compares the trailer's status counts and total with the records. See [Checks](#checks).
- **Inspects one record.** Select a row to see the raw line under a character ruler, with every field shaded and matched to its column. The record's problems are listed at the top; choose one to jump to its field.
- **Profiles fields.** The Fields view shows whether each field is required, conditional or optional, how often it's filled, how many distinct values it has, and the most common values.
- **Keeps the grid manageable.** Columns that are blank in every record (often Programs 2–6) stay hidden until you turn on **Empty columns**. Filler positions stay hidden until you turn on **Filler**.
- **Exports.** Copy the table for Excel (leading zeros kept), or save it as CSV.
- **Edits, one record or many.** Correct a single record, or set one value in every record your filters show. Every change can be undone, and History lists exactly what changed.

## Checks

Choose **Checks** on the toolbar for a list of every problem in the file, with the number of records for each. **Show records** filters the grid to just those records. The red and amber chips in the file summary do the same for all records with errors or warnings. In the grid, cells with a problem are shaded, and hovering one shows the message.

**Errors** are values NSC rejects, or that break a rule in NSC's file specification:

| Check | What it looks for |
|---|---|
| Required field is blank | SSN, first and last name, enrollment status, street, city, state, term dates, Directory Block Indicator, Program Indicator |
| Not one of NSC's codes | Any coded field, including NSC's state and country code lists |
| Not a valid date | Dates must be real calendar dates in YYYYMMDD form |
| Should be numbers only / Missing digits | SSNs, CIP codes, credential levels, program lengths and similar fields are digits only and fill every position. Excel often drops the leading zeros. |
| Status start date missing | Statuses W, G, A and D need the date the status began |
| Anticipated graduation date missing | Enrollment files need it for statuses F, Q, H, L and A |
| Term begins after it ends | Term Begin Date is after Term End Date |
| Status not allowed for advanced registration | P3 files allow only F, Q, H and L |
| Program fields missing | With Program Indicator Y, Program 1 needs all its fields. Programs 2–6 need theirs once their CIP code is filled. Weeks in the Title IV year are required when length is in months or weeks. |
| Program data with Program Indicator N | Program fields must be blank when the indicator is N |
| Credential level and special program don't agree | Special program A goes with credential level 02, B with 03, and P, T and U with 99 |
| Never-attended program has mismatched dates | For program status X, the status effective date must equal the program begin date |
| SSN on more than one record | NSC expects one detail record per student |
| Record isn't 1,250 characters / Record type isn't D1 | Fixed-width structure problems |
| Value longer than its field | A tab-delimited value has more characters than the field allows |

**Warnings** are worth a look. Several match warning codes on NSC's Error Resolution Report:

| Check | What it looks for |
|---|---|
| SSN looks like a placeholder | Starts with 000 or 666, has 00 in the middle or 0000 at the end, or is all one digit |
| Status starts after the term ends | NSC warning 80 |
| Status start date outside the term | NSC warning 264: an F, Q, H, L or A status start date before the term begins |
| Graduation date before the term | Anticipated graduation date is before the term begins |
| Program begins after the term ends | The program begin date is later than the term being reported |
| Program status date before the program began | Program status effective date is before the program begin date |
| Program status differs from campus status | An active program (F, Q, H, L or A) whose status doesn't match the campus-level status (FSA GEN-14-17) |
| Weeks filled for a program measured in years | Weeks in the Title IV year should be blank when length is in years |
| CIP year isn't 2020 | NSLDS uses the 2020 CIP list |
| Unlikely date of birth | The student would be under 10 or over 100 when the term begins |
| ZIP code doesn't look right | US ZIP codes are 5 or 9 digits |
| Foreign address without a country | State FO needs a country code |
| Email address doesn't look right | Not in the form name@example.edu |
| College student ID on more than one record | Two or more records share an ID |

These checks only see what's in the file. Some problems show up only when NSC compares your file with what it already has on record, such as warnings 1811–1816 for a changed program begin date. Those still appear only on NSC's Error Resolution Report. A clean Checks view means the file follows the published specification, not that NSC will raise no errors or warnings.

## Editing records

1. Select a row, then choose **Edit record** (or double-click the row).
2. Change values. Each one is checked as you type: field length, numbers and dates, and NSC's codes. The raw record and the record's list of problems update live, so you can see a problem clear before you apply.
3. Press **Enter** to apply, or **Esc** to cancel. Edited records and cells are marked in violet, and hovering an edited cell shows the old value.
4. Choose **Save as text file**. The saved file matches the original except for the characters you changed: the same header, record order, line endings and encoding. **The T1 trailer counts are recalculated** to match the records, so changing a student's status never leaves the trailer out of date.

For tab-delimited files, rows you didn't edit are written back exactly as they came in. In an edited row, only the columns you changed are rewritten.

In Chrome and Edge, saving opens a Save As dialog, and later saves write to the same file. Other browsers download the file.

Edits live only in the open tab until you save. Closing a file (the × on its tab) with unsaved edits asks whether to save first. Closing the whole browser tab shows only the browser's own generic "Leave site?" warning, because browsers don't allow pages to customize it.

### Changing many records at once

1. Search or filter so that only the records you want to change are shown. The Checks view's **Show records** is a quick way to do this.
2. Open the arrow on the column's header and expand **Change values in the N records shown**.
3. Type the new value and choose **Set**. Leave the box empty to blank the field. The value is checked the same way as a single edit, and you confirm the number of records before anything changes.

Records that already have the value are skipped. Records that no longer match your filters drop out of view.

### Undo, redo and the change list

Every change is one step: a record edit, a bulk change, putting a record back, or **Undo all edits**. **History** on the toolbar lists the steps, newest first, with what changed in each.

- Choose **Undo** on the latest step, or **Undo + N later** on an older step to go back further. Undone steps stay in the list with **Redo** until you make a new change.
- **Ctrl+Z** undoes and **Ctrl+Y** (or **Ctrl+Shift+Z**) redoes, as long as you're not typing in a box.
- **Copy change list for Excel** copies one row per changed value: record number, College Student ID, column, field, original value and new value. Use it to document your corrections.

History lasts while the file is open in the tab. If you undo past the point where you saved, the file shows as unsaved again.

## SSN masking

**Mask SSNs** is on by default. The grid, record panel, value lists and History then show only the last four digits of Student SSN and Previous SSN. Search and filters still match the full number. Exports, copied records and saved files always keep the full SSN. Turn the switch off to see full numbers on screen.

## The file layout

The built-in layout is NSC's enrollment reporting format, as published on Compliance Central under *File Formats & Field Values*:

| Record | Starts with | Contents |
|---|---|---|
| Header | `A3` or `P3` | School code (OPEID), branch code, academic term, standard report flag, certification date, reporting level |
| Detail | `D1` | One per student: 115 fields covering name, SSN, campus-level status and dates, address, demographics, flags, and up to six programs |
| Trailer | `T1` | Count of each enrollment status (F, Q, H, L, W, G, A, X, D), then total records (students plus the header and trailer) |

Programs 1–6 are repeated 43-character blocks starting at position 675 (Excel columns AW, BG, BQ, CA, CK and CU).

NSC's page lists the final filler as 316 characters, but positions 1077–1250 are 174 characters, and that is what the explorer uses.

Specification: <https://help.studentclearinghouse.org/compliancecentral/knowledge-base/excel-text-file-detail-records/>

## Adding your own layouts

Open **Layouts → New layout** and paste the rows of a record layout, one field per line: a name, then its start position and length (or start–end). Rows copied from Excel work. Optional tags at the end of a line: `[unused]`, `[dec=2]`, `[date]`, `[codes=ENRSTAT]`.

Layouts you make are saved in your browser only. To share them:

- **With a colleague:** Layouts → Share → Copy export, then they paste it and choose Import.
- **With everyone who uses your copy:** paste the export between the `office-layouts` script tags near the top of `index.html`.

## Limitations

- **Only the enrollment reporting file is built in.** Other Clearinghouse files, such as degree files and StudentTracker files, use different layouts. Add them under Layouts.
- **Excel workbooks can't be opened directly.** In Excel, choose Save As → Text (Tab delimited), then open that file.
- **Code meanings and checks** were transcribed by hand from NSC's published pages. The field positions were checked to chain with no gaps or overlaps, but spot-check any code label or rule you rely on.
- **The sample file is made up.** Its SSNs start with 9, which the Social Security Administration never issues, and a few records contain deliberate mistakes so the Checks view has something to show.
- Web fonts load from Google Fonts. Offline, the page falls back to system fonts.

## Running it locally

There is nothing to build or install. It is a single self-contained HTML file: download `index.html` and double-click it.

## Data and privacy

No server, no analytics, no uploads. The page reads files with the browser's file APIs. Layouts you create and your display settings (code labels, SSN masking, empty columns) are stored in the browser's local storage on your computer.
