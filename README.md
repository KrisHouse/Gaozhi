# 稿紙 — a Traditional Chinese writing app
 
A local writing app built around the experience of writing in Traditional Chinese. It is a single HTML file that runs in Chrome or Edge, and it keeps every book as plain Markdown files in a folder you choose, so your writing is easy to back up, sync and share.
 
## Project files
 
| File | What it is |
|---|---|
| `gaozhi.html` | The whole app: open it in Chrome or Edge |
| `稿紙-mac.png` | 1024 px app icon for the macOS launcher |
| `稿紙.ico` | Windows icon (16–256 px; small sizes use a bolder variant) |
| `README.md` | This file |
 
## Setup
 
### Mac (done: Automator launcher)
 
1. Keep `gaozhi.html` at a permanent path without spaces, e.g. `~/Documents/稿紙/gaozhi.html`.
2. Automator → New Document → **Application** → add **Run Shell Script**:
   ```
   open -na "Google Chrome" --args --app="file://$HOME/Documents/稿紙/gaozhi.html"
   ```
3. Save as **稿紙** in Applications and drag it to the Dock.
4. Icon: open `稿紙-mac.png` in Preview, copy it (⌘A, ⌘C), select 稿紙.app, press ⌘I, click the small icon, paste (⌘V).
### Windows
 
1. Keep `gaozhi.html` at a permanent path, e.g. `C:\Users\<name>\Documents\稿紙\gaozhi.html`.
2. Right-click the desktop → New → Shortcut, with this as the location:
   ```
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --app="file:///C:/Users/<name>/Documents/稿紙/gaozhi.html"
   ```
   Edge also works: `"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"`. Use one browser consistently on each computer, because the folder connection and settings are stored per browser.
3. Name it **稿紙**. Properties → Change Icon → choose `稿紙.ico`. Then pin it to Start or the taskbar (re-pin after changing the icon).
### First launch
 
Choose **選擇寫作資料夾** and pick your writing folder. To use the same books on several computers, put that folder in iCloud Drive, Dropbox or OneDrive and pick it once on each computer.
 
**先在瀏覽器裡試用** stores everything inside the browser instead of in files. It is meant for trying the app out; use 匯出 to get Markdown out of it.
 
### iPhone
 
The folder mode does not work on iPhone: iOS browsers don't support the File System Access API. The recommended approach is to keep the writing folder in iCloud Drive and edit the `.md` files directly in a Markdown app (Obsidian, iA Writer, 1Writer). Vertical mode, counts and 遺珠 are not available there, but the desktop app picks up the changes (see *Edits made elsewhere* below).
 
## Folder structure
 
```
寫作資料夾/
├── .稿紙設定.json     shared settings for all books: the 常用詞 list (hidden file)
└── 書名/
    ├── 稿件/          manuscripts (.md)
    ├── 筆記/          notes (.md), including any conflict copies
    ├── 遺珠.md        cut paragraphs
    └── .稿紙.json     daily word-count history (hidden file)
```
 
## Manuscript format
 
| In the editor | In the Markdown file |
|---|---|
| Paragraph | One line, separated from the next by a blank line |
| Section break ◇　◇　◇ | `* * *` |
| Chapter heading | `# 第一章　標題` |
 
- Paragraph indentation is display-only; it is not written into the file.
- A paragraph that starts with `#`, or looks like a break, is escaped with `\` so it isn't misread.
- On load the app also accepts three or more repeats (spaces allowed) of `*`, `-`, `_`, `◇`, `◆`, `＊`, `※`, `☆`, `★`, `○` or `●` as section breaks, and any `#`–`######` heading as a chapter.
## Features
 
### Writing
 
- **Enter rules**
  - 1× Enter: new paragraph.
  - 2× Enter (Enter on the empty line): section break.
  - 3× Enter: the break becomes a new chapter, auto-numbered 第N章 with the title selected for typing.
  - Enter on the empty first line of a manuscript creates a chapter heading directly.
  - Enter inside a chapter title moves to the body.
  - Backspace reverses any of these steps.
- **橫排 / 直排**: horizontal left-to-right, or vertical right-to-left like a printed book. In 直排 the mouse wheel scrolls sideways.
- **Toolbar**: 明體 or 黑體, font size, two-character paragraph indent, 標示 (常用詞 highlighting on/off) and 配色.
- **配色 (themes)**: 跟隨系統 (light or dark with the system), 宣紙, 茶箋 (sepia), 竹青 (green), 霧藍 (blue-grey), 墨夜 (dark), 深海 (dark blue), 純黑 (black).
  - The whole interface follows the theme, including the 每日字數 chart and the app window's title bar.
  - The choice is saved per browser, like the font and size settings.
- **Punctuation palette**: Taiwan-style marks: ，。、；：！？「」『』（）《》〈〉……——‧ Paired marks wrap the selected text.
- **Paste**: plain text only. Pasted Markdown keeps its chapters and breaks.
- **Undo / redo**: the app keeps its own history, so it works correctly with the custom Enter behaviour.
- **IME**: Enter while choosing a candidate (注音, 倉頡, etc.) is left to the input method.
- **Saving**: automatic about 1 second after you stop typing; Ctrl/⌘‑S saves immediately.
### Counts
 
- Character counts are shown **with** and **without** punctuation for the current chapter (本章), the manuscript (本稿), the whole book (全書) and the selection (選取).
- Whitespace is never counted; chapter titles are excluded.
- The sidebar shows a table of contents with a count for each chapter; click a chapter to jump to it.
### Books, manuscripts and notes
 
- Create books, manuscripts and notes; rename or delete files.
- Notes use a plain text area, which also follows 橫排 / 直排.
- **匯出** downloads the current file; **匯出全書** downloads all manuscripts merged into one `.md`.
### 遺珠 (cut paragraphs)
 
- **Sending text**: **Alt+J** (⌥J on Mac) or the toolbar's **送遺珠** moves text out of the manuscript.
  - A cursor with no selection sends the whole paragraph.
  - A selection sends exactly the selected text, even across paragraphs; chapter titles are never moved.
- **Each entry** records the time, its character count and a source link (e.g. `正文 › 第一章　雨夜`). Clicking the link opens the manuscript at that chapter and highlights it briefly.
- **The 遺珠 view** (工具 → 遺珠) lists entries newest first, with 複製 and 刪除 buttons and a box for adding passages by hand.
- **File format**: in `遺珠.md`, each entry has a `## 2026-10-15 14:32` heading, then a source line `來源：[正文 › 第一章](<稿件/正文.md>)`, then its paragraphs. The link also works in other Markdown editors.
- **Renaming**: renaming a manuscript updates its 遺珠 links. If a chapter was renamed, the link opens the manuscript and says the chapter wasn't found.
- **Undo**: Ctrl/⌘‑Z restores the text to the manuscript, but the 遺珠 copy stays.
### 每日字數 (daily word count)
 
- **What counts**: the net change in the book's total character count each day.
  - Deleted text is subtracted.
  - Moving text to 遺珠 and deleting a whole manuscript don't count against you.
  - Edits made outside the app are not counted.
- **Status bar**: shows `今日 +N ／ 目標` for the current book. Click it, or open 工具 → 每日字數, for the full view.
- **The full view**: today, the last 7 and 30 days, the streak, today's total across all books, a 30-day chart with a goal line and a 7-day list. It also has a daily goal setting and a 含標點 / 不含標點 switch.
- **Storage**: history is kept per book in `.稿紙.json`, so it syncs along with the folder.
  - The file is written alongside every manuscript save, and at least every 2 seconds while you write.
  - Today's record is also backed up instantly in the browser. If the window closes before the file is written, the count is restored from the backup on the next launch.
### 常用詞 (overused words)
 
- **The list**: words and phrases you tend to overuse. It starts with 20 common ones (然後, 突然, 其實, 緩緩, 微微, 彷彿, …) that you can remove or add to.
  - The list is shared by all books and stored in `.稿紙設定.json` in the writing folder, so it syncs with the folder.
- **Highlighting**: every occurrence in the open manuscript gets a background colour, updated as you type.
  - A word directly after another listed word gets a slightly darker shade, so 只是緩緩 reads as two words.
  - The highlight is display-only. The text, saving, undo and IME input are not affected.
  - Toggle it with **標示** in the toolbar or the checkbox in the 常用詞 view.
  - The status bar shows `常用詞 N 處` for the open manuscript; click it to open the 常用詞 view.
- **The 常用詞 view** (工具 → 常用詞):
  - Each listed word shows how many times it appears in the whole book. Hover for the rate per 10,000 characters; red marks 5 or more per 10,000.
  - Add several words at once, separated by spaces or 、. Remove with ✕. 加入預設詞語 restores the starter list; 清空清單 empties it.
  - **自動找出的重複詞** lists two- to four-character phrases that appear at least 3 times in the book, most frequent first, with ＋ 加入 and 忽略. Ignored phrases (e.g. character names) are remembered and can be restored.
### Edits made elsewhere (e.g. on the phone)
 
- **When it checks**: every 5 seconds, when the window regains focus, and right before every save.
- **Changed only elsewhere**: the app reloads the file silently and keeps your place in the text.
- **Changed in both places**: the app asks which version to keep. The other version is saved to 筆記 as `衝突副本：<名稱>（本機／外部 MMDD-HHmm）.md`, so nothing is lost.
- **Other files**: totals and file lists update when other files change or new files appear.
- **Deleted or renamed elsewhere**: if the open file was deleted or renamed elsewhere, you get a warning; continuing to type recreates it.
- These checks apply only to folder mode; browser-storage mode has no files to watch.
## Keyboard shortcuts
 
| Keys | Action |
|---|---|
| Enter / ×2 / ×3 | Paragraph / section break / new chapter |
| Alt+J (⌥J) | Send to 遺珠 |
| Ctrl/⌘‑Z, Ctrl/⌘‑Shift‑Z or Ctrl‑Y | Undo / redo |
| Ctrl/⌘‑S | Save now |
 
## Limitations
 
- **Browsers**: folder mode needs Chrome or Edge on a computer. Firefox, Safari and all iPhone browsers can only use browser storage.
- **Fonts**: the interface loads Noto Serif TC and Noto Sans TC from Google Fonts. Offline it falls back to system fonts (新細明體, 微軟正黑體, 宋體-繁, etc.).
- **Undo and 遺珠**: undoing a 遺珠 move restores the manuscript but doesn't remove the 遺珠 entry.
- **常用詞 highlighting**: works in manuscripts only, not in notes. It needs the CSS Custom Highlight API (Chrome or Edge 105 and later).
- **Writing done elsewhere**: the daily count can't attribute phone writing to the day it happened, so phone writing isn't counted at all.
- **Sync timing**: iCloud and other sync services take a moment, so the app can only react once a change actually arrives.
- **Large books**: books are read in full when opened. This is fine for normal novels, but very large books will open more slowly.
## Possible next steps (not done)
 
- Count writing done outside the app (e.g. on the phone) in 每日字數, credited to the day the app next opens.
- Sync through Dropbox or Google Drive, so the full app works on iPhone in the browser.
- Package as a native iOS app (Capacitor) that reads the iCloud Drive folder; needs a Mac with Xcode and an Apple Developer account.
- Improve the phone layout: a crowded toolbar and untested 注音 Enter behaviour on iOS.
## Change log
 
All on 2026-10-15.
 
1. **Initial app.** Single-file HTML app with folder storage via the File System Access API and a browser-storage trial mode.
   - Books contain manuscripts and notes, all stored as Markdown.
   - Horizontal and vertical writing modes.
   - Enter rules for paragraphs, section breaks and chapters.
   - Character counts with and without punctuation.
   - Table of contents, punctuation palette, custom undo, autosave and export.
2. **Fix: chapter heading at the very beginning.** Enter on the empty first line of a manuscript now creates a chapter heading.
3. **遺珠.** Send-to-遺珠 with Alt+J / ⌥J and a toolbar button, source links back to the chapter, a 遺珠 view, manual adding, and link updates when a manuscript is renamed.
4. **每日字數.** Per-book daily net count, status-bar display, a stats view with goal, streak, 30-day chart and all-books total, and 含標點 / 不含標點.
   - Moving text to 遺珠 and deleting a manuscript don't count against the day.
5. **Discussed iPhone use.** Recommended iCloud Drive with a Markdown app on the phone; noted iOS limitations.
6. **External-change detection.** Silent reload when only the other side changed, a conflict prompt with a saved copy when both changed, and recount and refresh for other files.
   - Outside edits are excluded from the daily count.
7. **App launchers.** macOS Automator app (set up) and Windows shortcut instructions, both using Chrome's `--app` mode.
8. **Icons.** `稿紙-mac.png` (macOS style) and `稿紙.ico` (Windows, bold small sizes): the character 稿 in a red manuscript-paper square.
9. **Fix: daily count reset to 0 after closing and reopening.**
   - **Cause**: `.稿紙.json` was saved 2 seconds after the last change, and every keystroke restarted that wait. During steady writing it was never written, and the final write while the window closed was cut off. On reopening, today had no record, so it started again from 0.
   - **Fix**: the file is now written with every manuscript save and at least every 2 seconds. Today's record is also backed up instantly in the browser and restored if the file missed it.
   - Tested in Chromium: before the fix, +20 became 0 after reopening; after the fix it stays +20, in both folder and browser-storage mode.
10. **配色.** Eight themes in a toolbar menu, also colouring the app's title bar.
11. **常用詞.** A shared overused-words list with live highlighting in the manuscript, a status-bar count, per-book counts and automatically found repeated phrases.
12. **Fix: a paragraph that looks like a break lost its escape on reload.** (2026-10-16)
    - **Cause**: a paragraph such as `◇◇◇` was saved as `\◇◇◇`, but on load only `\` before `#`, `*`, `-` or `_` was removed, so the backslash showed in the text.
    - **Fix**: the unescape now covers every character the break detector accepts (`◇ ◆ ＊ ※ ☆ ★ ○ ●` too). Checked with a save/load round trip. The accepted break characters are now listed under *Manuscript format*.
