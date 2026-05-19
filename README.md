# Enhanced Text Compare Tool

A self-contained, single-file HTML tool for comparing two pieces of text side by side, reviewing changes word by word, and saving/restoring your session.

---

## Getting Started

No installation required. Just open [`text-compare.html`](https://leoncoolmoon.github.io/compare) in any modern browser (Chrome, Firefox, Safari, Edge).

---

## Features

### Text Input
- Paste text directly into either box, or drag and drop a plain text file onto it
- Character count updates live as you type
- Both boxes are independent — selecting text in one keeps the highlight visible after you click into the other

### Comparing
- Click **Compare Texts** to run a word-level diff
- Changed words are highlighted inline:
  - 🟢 **Green** — added text
  - 🔴 **Red strikethrough** — removed text
  - 🟡 **Yellow** — abbreviated form
  - 🔵 **Blue** — expanded form

### Reviewing Changes
Click any highlighted change to get a floating menu with two options:

| Action | Effect |
|--------|--------|
| ✓ Accept | Keeps the new (modified) version |
| ✗ Reject | Keeps the original version |

Use **Accept All** or **Reject All** to process every change at once.

### Copying the Result
Click **Copy Final Text** to copy the resolved text to your clipboard.  
Unresolved changes default to keeping the original text.

---

## Import / Export

Your session can be saved and restored as a JSON file, preserving both texts, all accept/reject decisions, and any text highlights.

### Export
Click **Export JSON** at any time — in the editing view or the compare view.  
A file named `compare-session.json` will be downloaded.

### Import
Click **Import JSON** to open the import panel. You can:
- **Drag and drop** a `.json` file onto the drop zone
- **Click** to browse for a file

If the session was exported from the compare view, the diff will be re-run automatically and all previous accept/reject decisions will be restored.

### JSON Structure

```json
{
  "version": 1,
  "text1": "original text…",
  "text2": "modified text…",
  "highlight1": { "s": 0, "e": 0 },
  "highlight2": { "s": 12, "e": 45 },
  "inResultView": true,
  "changes": [
    {
      "id": "cg-0",
      "type": "replace",
      "oldValue": "foo",
      "newValue": "bar",
      "state": "accepted"
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `version` | Format version (always `1`) |
| `text1` | Contents of the original text box |
| `text2` | Contents of the modified text box |
| `highlight1/2` | Character indices of the frozen selection (`s` = start, `e` = end) |
| `inResultView` | Whether the compare view was active when exported |
| `changes` | Array of every detected change and its review state |
| `changes[].type` | `replace`, `delete`, `insert`, or `abbreviation-expand` |
| `changes[].state` | `pending`, `accepted`, or `rejected` |

---

## Keyboard & Navigation

| Action | How |
|--------|-----|
| Switch between text boxes | Click, or Tab / Shift+Tab |
| Scroll long text | Mouse wheel inside the text box |
| Close floating menus | Click anywhere outside them |

---

## Notes

- The tool runs entirely in your browser — no data is sent anywhere
- For very large texts the diff runs asynchronously to keep the page responsive; a loading indicator appears while it works
- The same file can be used offline
