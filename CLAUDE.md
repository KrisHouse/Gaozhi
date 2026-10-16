# 稿紙 — project guide for Claude

A local writing app for Traditional Chinese. **The whole app is one file: `gaozhi.html`** (HTML + CSS + JS inline). There is no build step, no package manager and no server. `README.md` is the user-facing documentation and the source of truth for behaviour; read it before changing features.

## Hard constraints

- **Stay single-file.** All code goes in `gaozhi.html`. No bundlers, no npm dependencies, no external scripts. The only external resource is Google Fonts (Noto Serif TC / Noto Sans TC), with system-font fallbacks.
- **Runs from `file://`** in Chrome/Edge `--app` mode (see README → Setup). Don't add anything that needs an HTTP origin.
- **Storage** is the File System Access API (folder mode) or browser storage (trial mode: `localStorage`, plus `indexedDB` for the remembered folder handle). Every feature must work in both, or degrade cleanly in browser-storage mode.
- **The user's books are not in this repo.** The writing folder lives elsewhere (often iCloud Drive). Never read, write or move files in it unless asked.

## File formats are a compatibility contract

Users edit the same `.md` files in other apps (Obsidian, iA Writer on iPhone). Don't change these formats without migration and a README update:

- Manuscripts: one paragraph per line, blank line between; `* * *` = section break; `# 第一章　標題` = chapter. Indentation is display-only. Paragraphs starting with `#` or looking like a break are escaped with `\`. Loader also accepts three or more repeats (spaces allowed) of `*`, `-`, `_`, `◇`, `◆`, `＊`, `※`, `☆`, `★`, `○`, `●` as a break, and any `#`–`######` heading as a chapter. The escape set in `escLine`/`unesc` must stay in step with the `BREAK` characters.
- `書名/遺珠.md`: entries headed `## YYYY-MM-DD HH:mm`, then `來源：[稿件 › 章](<稿件/檔名.md>)`, then paragraphs.
- `書名/.稿紙.json`: daily count history. `寫作資料夾/.稿紙設定.json`: shared 常用詞 list.
- Conflict copies: `筆記/衝突副本：<名稱>（本機／外部 MMDD-HHmm）.md`.

## Behaviour that is easy to break

- **Enter rules** (paragraph / break / chapter, Backspace reverses) and the **custom undo stack** are tightly coupled. Test both together.
- **IME**: never act on Enter while `isComposing` (注音, 倉頡). Test with composition events, not only plain keydown.
- **Counts**: whitespace never counts; chapter titles excluded; with/without punctuation. 每日字數 is net change, but moves to 遺珠, manuscript deletion and external edits must not count.
- **Daily count persistence**: `.稿紙.json` is written with every manuscript save and at least every 2 s, plus an instant browser backup. This fixed a real data-loss bug (change log #9); don't revert to a debounce that restarts on every keystroke.
- **External-change detection**: polls every 5 s, on focus and before every save. Conflicts must always keep both versions.
- **常用詞 highlighting** uses the CSS Custom Highlight API and must never touch the DOM text (saving, undo and IME depend on that).
- **直排** (vertical-rl): check layout, caret movement and wheel-to-horizontal scrolling whenever editor CSS changes.
- **Themes**: new UI must use the theme's CSS variables, including the 每日字數 chart and `theme-color`.

## Testing

No test suite yet. Verify changes in Chromium, e.g. with Playwright (`pip install playwright --break-system-packages && playwright install chromium`), loading `file://…/gaozhi.html`. Browser-storage mode is the easiest to script; folder mode needs a real folder picker, so check it by hand. For bug fixes, reproduce first, then confirm the fix, as was done for change log #9.

## Conventions

- UI text is Traditional Chinese with Taiwan-style punctuation (「」, full-width ，。, 全形空格　).
- After any user-visible change, update `README.md`: the relevant Features section and a new numbered entry in the Change log (date + what changed; for bugs, cause and fix).
- Keep `README.md` → *Possible next steps* current when something there gets done.
