# ForgeGrid

A Phase 1 Chrome prototype of an enterprise data and order workspace. Create your own **categories** (programs, dashboards, references — you name them), sheets, custom columns, lookup links, and formulas. Drop a PDF, screenshot, or EDI file on the left and enter rows on the right. Export a real multi-sheet Excel workbook in one click.

Everything runs in the browser. There is no server. Data lives in `localStorage` on this machine.

## Open it

Double-click `index.html` and it will open in Google Chrome (an internet connection is required the first time so Tailwind, Vue 3, and SheetJS can load from CDNs).

Or serve the folder:

```bash
python3 -m http.server 8764 --bind 127.0.0.1
```

Then visit `http://127.0.0.1:8764`.

## First launch

1. Name the workspace.
2. Set a 4–8 digit Architect PIN (you choose it; it never leaves this browser).
3. Leave **Load sample operations workspace** checked to get Customers, Products, Orders, and Line Items already linked together.

You start in **Operator Mode**. Structure is locked. Switch to **Architect Mode** with the PIN when you need to add tables, columns, lookups, or formulas.

## What to try

- **Orders** and **Line Items** — customer and product lookups, a `{Qty} * {Unit Price}` formula, and rollups for line count and order total.
- **Look at the cell above (this cell only)** — type a date (or anything) in one row. Add a row under it; that new row does **not** inherit a formula. Right-click the new cell → **Look at the cell above (this cell only)**. Only that cell follows the one above; the rest of the column stays normal to type in. Right-click **Type in this cell again** to stop following. Or type `=B1`.
- **Search & link** — a column type with a search button, a dropdown, and type-ahead. Architects point it at another sheet, choose the field to show, and search either **one column** or the **whole row**. Type `br` and **Brondby IF** appears as soon as it matches (sample Customers includes that name). Arrow keys move the highlight; Enter picks it. Lookup / pick links use the same dropdown. If this browser already has an older workspace, Architect → Backup → Workspace settings → **Replace with sample workspace** to load that demo.
- **XLOOKUP links** — on a **Lookup / link** column, choose **XLOOKUP — match a typed key**. Type a value in this column or another (for example Item No `1`). ForgeGrid finds the related row where that column equals `1` and fills the link plus connected fields (autofill price, live **Linked field** columns). Misses can stay blank or show `#N/A`. Sample Line Items: type `1` → Widget A, Hardware, $12.50.
- **XLOOKUP formula** — `XLOOKUP({Item No}, "Products", "Item No", "Unit Price")`.
- **Categories** — the sidebar is not one pile of tables. Architects click **New category** to add as many as they want (programs, dashboards, references — you name them). Each category has a chevron to **expand or collapse**. Kind is Program, Dashboard, or References; the name is yours. Sample uses an **Operations** category only — extra programs or dashboards are created the same way, not hard-coded. **Copy** a category or sheet, then **Paste** in the header (new category) or on another category (sheets land there). Drag a category to reorder. Drag a sheet onto another category to **move** it; hold Ctrl/Alt while dropping to **copy**. When a sheet moves or is pasted into a different category, its description is restamped with that sheet’s kind and the category name (for example `Program · Operations`). Hide the whole left workspace with the chevron on that panel (or the grid icon in the header) so the rows have more room; click the thin **Workspace** strip to bring it back.
- **Edit a sheet** — Architects click the sheet name in the toolbar, **Edit sheet**, or the pencil in the sidebar. Change **name**, **colour** (swatches), and **kind** (Program, Dashboard, or References) independently of the category it sits in. Changing kind updates the description stamp.
- **Dashboard idea** — a Dashboard category opens a placeholder for later stats. Add sheets there when you want overviews or inventories; nothing is pre-built.
- **Columns like Excel** — drag a header to reorder. Drag the right edge to resize as narrow as you like; the header name wraps instead of forcing a wide column. Resizing one column does not stretch the others to fill the window — empty space on the right is fine. Widths save with the workspace.
- **Lock a row** — click the lock icon (open = unlocked, closed gold = locked). Click again to unlock. Drag the right edge of the **#** column to resize it.
- **Row history** — clock on the row. Each change stores previous (rose, strikethrough) and new (teal) values; the cell shows the same colours.
- **Select cells like Excel** — click a cell to type or search (no double-click). Click-drag or Shift-click a block. Click a **row number** to select the row (Shift-click or drag down the # column for more). Click a **column header** to select the column (Shift-click or drag across headers for more). Click **#** for the whole sheet. **Ctrl+A** selects all. The ↕ on a header sorts; the grip reorders columns. **Right-click** opens Copy, Paste, Duplicate, Fill down, Clear, Insert row, Select row/column, Lock, and Delete — not the browser menu. **Ctrl+C** / **Ctrl+V** / **Ctrl+D** (duplicate rows). **Delete** clears cells; **Ctrl+Delete** deletes rows. Footer **Sum**, **Avg**, **Count**. Calculated cells and locked rows are skipped. Operators do not see **+ Row** until the sheet has at least one column.
- **Delete a row** — trash on the row, **Delete row** in the toolbar, or **Ctrl+Delete**. Confirm in the dialog. Delete on the keyboard still only clears cells.
- **This column, row above** — `{This[-1]}` is the same column one row above (Excel `A1` when you are on `A2`). `{This[-1]}+1` or `ABOVE()+1` increments it. Sample **Line #** does this (1, 2, 3…). **Running Total** is `COALESCE({This[-1]},0)+{Line Total}`. Ranges: `SUM({Qty[-2:0]})` is this row and the two above. In the formula editor, use the **Row offset** control (`-1` is the row above) and **Insert cell + 1**. The formula runs on every row in that column.
- **Same-column formulas** — on a number or date column, set `{This}+1` so the value you type in that cell is the input. Dates use day/month/year: `01/01/2026+1` → `02/01/2026`. Sample **Hold Until** does this; **Follow-up** is `{Order Date}+1`.
- **Fields** — Architects hide a column with the eye on the header, then **Fields → Unhide**. **Hide row** is Architect-only. Operators do not see hide controls; they can still unhide from Fields if a column was already hidden.
- **Documents** — open the split-screen, drop a PDF/image/EDI file (or paste a screenshot with Ctrl+V), and use **Entry form** on the right. PDFs hide the thumbnail strip and fit to page width. **Read large** opens a full-window reader (zoom, page width, Escape to close). **New window** opens Chrome’s own PDF viewer so you can put the order on a second screen. Drag the splitter to give the document more width.
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
