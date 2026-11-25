# ForgeGrid

A Phase 1 Chrome prototype of an enterprise data and order workspace. Create your own **categories** (programs, dashboards, references — you name them), sheets, custom columns, lookup links, formulas, and **client order recipes**. Drop a PDF, screenshot, or EDI file on the left and enter rows on the right. Export a real multi-sheet Excel workbook in one click.

## How it is split (do not mix)

- **Paper** — the grid is a real spreadsheet canvas (select, fill, copy/paste, drag columns, resize). That is how you move around cells, as in Excel.
- **Meaning** — the **column panel** (`+ Column`, double-click a header) is how you set type, lookup, XLOOKUP, formula, rollup, search. **+ Row** adds a record that inherits that column.
- New rows inherit the **column**. A cell formula (`=B2`) is an exception on that cell, not a new kind of column.
- Operators enter data. Architects change structure. Operators do not see A/B/C letters.

Everything runs in the browser. There is no server. Data lives in `localStorage` on this machine.

## Open it

Double-click `index.html` and it will open in Google Chrome (an internet connection is required the first time so Tailwind, Vue 3, SheetJS, and PDF.js can load from CDNs).

Or serve the folder:

```bash
python3 -m http.server 8764 --bind 127.0.0.1
```

Then visit `http://127.0.0.1:8764`.

## First launch

1. Name the workspace.
2. Set a 4–8 digit Architect PIN (you choose it; it never leaves this browser).
3. Leave **Load sample operations workspace** checked to get Customers, Products, Orders, Line Items, and a **Rema** recipe already linked together.

You start in **Operator Mode**. Structure is locked — including creating or editing recipes. Switch to **Architect Mode** with the PIN (gold **Architect** button in the header) when you need to add tables, columns, lookups, formulas, or recipes. Operators can still pick a recipe and Fill sheet.

## What to try

- **Orders** and **Line Items** — customer and product lookups, a `{Qty} * {Unit Price}` formula, and rollups for line count and order total.
- **Architect grid addresses** — in Architect Mode, headers show **A, B, C** and the selected cell shows **B2**. Operators do not see letters. Type **=** on a cell, then click another cell or a header to insert `{Qty}` (same row) or `B2` (another row). `Customers!B2` works across sheets. Enter saves that formula on **this cell**; the column default stays on other rows.
- **Set type / rule for this cell** — right-click a cell. Opens the **same panel as New column**. Type is the rule (date, lookup, formula…). It applies to that cell only; the column stays the default for other rows. **Use column type again** puts it back.
- **Copy / paste / duplicate / fill down** — copies the cell **rule** as well as the text. Formula addresses like `A1` shift. Column formulas copy when you copy a whole column (Architect).
- **Paste from Excel** — Architects can paste a block including the **header row**. On an empty sheet (or when the paste is wider than the sheet) ForgeGrid creates columns from those headers and extra rows as needed. Types are guessed from the values (number, date, currency, email…). **Double-click a header** (or the pencil) to change name, type, lookup, formula, or any other column option. Operators paste values only into columns that already exist.
- **Search & link** — a column type with a search button, a dropdown, and type-ahead. Architects point it at another sheet, choose the field to show, and search either **one column** or the **whole row**. Type `br` and **Brondby IF** appears as soon as it matches (sample Customers includes that name). Arrow keys move the highlight; Enter picks it. Lookup / pick links use the same dropdown. If this browser already has an older workspace, Architect → Backup → Workspace settings → **Replace with sample workspace** to load that demo.
- **XLOOKUP links** — on a **Lookup / link** column, choose **XLOOKUP — match a typed key**. Type a value in this column or another (for example Item No `1`). ForgeGrid finds the related row where that column equals `1` and fills the link plus connected fields (autofill price, live **Linked field** columns). Misses can stay blank or show `#N/A`. Sample Line Items: type `1` → Widget A, Hardware, $12.50.
- **XLOOKUP formula** — `XLOOKUP({Item No}, "Products", "Item No", "Unit Price")`.
- **Categories** — the sidebar is not one pile of tables. Architects click **New category** to add as many as they want (programs, dashboards, references — you name them). Each category has a chevron to **expand or collapse**. Kind is Program, Dashboard, or References; the name is yours. Sample uses an **Operations** category only — extra programs or dashboards are created the same way, not hard-coded. **Copy** a category or sheet, then **Paste** in the header (new category) or on another category (sheets land there). Drag a category to reorder. Drag a sheet onto another category to **move** it; hold Ctrl/Alt while dropping to **copy**. When a sheet moves or is pasted into a different category, its description is restamped with that sheet’s kind and the category name (for example `Program · Operations`). Hide the whole left workspace with the chevron on that panel (or the grid icon in the header) so the rows have more room; click the thin **Workspace** strip to bring it back.
- **Edit a sheet** — Architects click the sheet name in the toolbar, **Edit sheet**, or the pencil in the sidebar. Change **name**, **colour** (swatches), and **kind** (Program, Dashboard, or References) independently of the category it sits in. Changing kind updates the description stamp.
- **Dashboard idea** — a Dashboard category opens a placeholder for later stats. Add sheets there when you want overviews or inventories; nothing is pre-built.
- **Columns** — drag a header to move it; drag the edge to resize. Widths save with the workspace. Do not insert columns inside the sheet — use **+ Column** or paste Excel headers, then edit the header.
- **Lock a row** — click the lock icon (open = unlocked, closed gold = locked). Click again to unlock. Drag the right edge of the **#** column to resize it.
- **Row history** — clock on the row. Each change stores previous (rose, strikethrough) and new (teal) values; the cell shows the same colours.
- **Spreadsheet canvas** — the grid is Excel-style (select, type, fill handle, copy/paste, drag headers to move columns, resize). **+ Row** still adds a ForgeGrid record. **Double-click a header** (Architect) opens the same column panel as **+ Column** — type, lookup, formula. Right-click for copy, paste, sort, lock, delete. Search / link / attachment cells open the ForgeGrid editor, not a plain text box.
- **Delete a row** — trash on the row, **Delete row** in the toolbar, or **Ctrl+Delete**. Confirm in the dialog. Delete on the keyboard still only clears cells.
- **This column, row above** — `{This[-1]}` is the same column one row above (Excel `A1` when you are on `A2`). `{This[-1]}+1` or `ABOVE()+1` increments it. Sample **Line #** does this (1, 2, 3…). **Running Total** is `COALESCE({This[-1]},0)+{Line Total}`. Ranges: `SUM({Qty[-2:0]})` is this row and the two above. In the formula editor, use the **Row offset** control (`-1` is the row above) and **Insert cell + 1**. The formula runs on every row in that column.
- **Same-column formulas** — on a number or date column, set `{This}+1` so the value you type in that cell is the input. Dates use day/month/year: `01/01/2026+1` → `02/01/2026`. Sample **Hold Until** does this; **Follow-up** is `{Order Date}+1`.
- **Fields** — Architects hide a column with the eye on the header, then **Fields → Unhide**. **Hide row** is Architect-only. Operators do not see hide controls; they can still unhide from Fields if a column was already hidden.
- **Client recipes** — Unlock **Architect**. Click **Docs** in the header, then **Open file** and choose a sample PDF (or **Move to other monitor** for a real window you can drag onto a second screen). Open **Recipes**, drag the title bar to move the dialog, or **Move to other monitor** to undock it. Name the client, pick your sheet, then **Teach on PDF**. In the gold bar pick a column and click that word on the PDF. Tick **Repeats on each line**, pick Item No, then click **every unique product number in that same PDF column** (306810, 306806, 306805) — extra clicks on that column are added, they do not overwrite. Then map Qty and Price on the first product line. Upload still fills each unique SKU’s own quantity and price. Tick **Skip crate / box lines** and set **ending with 15** so Europool rows like 379015 are ignored. Leave **One row per unique product** on so a repeated SKU is not written twice. After a click, open **Recipes** on that mapping and choose what to put in the cell: the PDF text, a **lookup / link** (REMA → customer row, 306832 → product), copy another field from that match into extra columns, or a value you type. **Save**. Operators can attach a PDF on the row’s attachment column (sample **Orders** → **Source file**): ForgeGrid detects the recipe from the PDF text and fills the other cells on that row, plus line items. **Search / link** columns still work: PDF text like `REMA` is matched to the related row (same as typing in the search box) and the cell stores the link, not the raw letters. You can still open Docs, pick a recipe, and **Fill sheet** to create a new row. Regex, lookups, and fixed values still work in the recipe editor.
- **Documents** — open the split-screen, drop a PDF/image/EDI file (or paste a screenshot with Ctrl+V), and use **Entry form** on the right. PDFs hide the thumbnail strip and fit to page width. **Read large** opens a reader you can drag by the title bar (zoom, page width, Escape to close). A dialog cannot leave this Chrome window. **Move to other monitor** opens a popup window (not a new tab) you can drag onto a second screen; clicks on that PDF still teach the recipe. Allow pop-ups once. Drag the splitter to give the document more width.
- **Attachment columns** — Architects add a column type **Attachment**. A filled paper thumbnail means a file is attached (hover for the name). The small line icons beside it open the file beside the row, open a new window, replace, or remove. An empty dashed paper with a plus attaches a file. Sample **Orders** has **Source file**. Files stay in this browser (IndexedDB); a JSON backup stores the file name, not the bytes.
- **Excel** — downloads one worksheet per table, plus `_Schema` and `_Audit`.
- **Backup** — JSON export/import restores the whole workspace, including the audit log.

Documents stay in memory for the session only. Tables, columns, formulas, rows, and audit events persist across reloads.

## Roles

| Mode | Can do |
| --- | --- |
| Operator | Enter and edit rows, copy to clipboard, split-screen document entry, Excel export, JSON backup |
| Architect | Everything in Operator, plus create/edit/delete/copy/move categories and sheets (name, colour, kind), columns, lookup links, formulas, rollups, and the PIN |

If you forget the PIN, use **Reset workspace** from Architect settings after exporting a JSON backup. Reset wipes local data.

## Phase 2 (not in this file)

A later production stack is planned around Streamlit, Pydantic, SQLAlchemy/SQLite, simpleeval, PyMuPDF, and openpyxl. This `index.html` file is the workflow prototype to approve first.
