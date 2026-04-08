# Claude Office

**The Universal Document Standard for AI**

## The Problem Nobody Talks About

Claude can use Microsoft Office. Claude can use Google Drive. Claude can use Apple Pages. Claude can use LibreOffice. Claude can juggle seventeen different document formats while simultaneously debugging your code and telling you why your architecture is wrong.

But here's the thing nobody stopped to ask:

**What's the *best* app for Claude to use across literally every device, every system, every ecosystem?**

Spoiler: It's not any of the walled gardens everyone's been shouting about.

---

## Meet LibreOffice

LibreOffice is what document software looks like when it was designed to actually *work* instead of lock you in.

### Why LibreOffice Wins

| Problem | Microsoft | Apple | Google | LibreOffice |
|---------|-----------|-------|--------|-------------|
| **Works on Linux?** | "Enterprise license required" | lmao | Sort of | Yes |
| **Works on Windows?** | If you pay annually | N/A | Kinda | Yes |
| **Works on Mac?** | If you pay annually | If you like broken formatting | Kinda | Yes |
| **Formatting stays the same across devices?** | Narrator: *It didn't* | "Why is everything centered" | "Where did my fonts go" | Actually does |
| **File size bloat?** | Docx = 2MB for "Hello World" | Pages = proprietary nonsense | Ugh | Sensible |
| **Open standard format?** | OOXML (theoretically open, practically corporate) | .pages (proprietary) | Locked to Google | ODF (real open) |
| **Headless CLI mode?** | No | No | No | `soffice --headless` |
| **ODF-native?** | Converts with losses | N/A | Converts with losses | Native, zero conversion |

---

## Deprecated: OnlyOffice

We used OnlyOffice. We stopped. Here's why:

**OnlyOffice is OOXML-native.** When you open an .odt file in OnlyOffice, it silently converts it to OOXML internally. That conversion introduces formatting losses - paragraph backgrounds render wrong, style inheritance breaks, borders appear or disappear. You're generating ODF documents and the app is fighting the format the entire time.

**OnlyOffice has no headless mode.** Zero CLI support. Every QA check requires opening the GUI, taking screenshots with computer-use, scrolling page by page. It's slow, it takes over the cursor, and it can't be scripted.

**OnlyOffice has no style inheritance.** You can't create a paragraph style that inherits from another style. LibreOffice has full style inheritance - the way ODF was designed to work.

LibreOffice is ODF-native, has full headless CLI support, and doesn't fight the format. The switch was immediate and permanent.

---

## The Headless Pipeline

This is the killer feature. No GUI. No cursor. No computer-use. Fully scriptable document QA:

```bash
# Generate your .odt (Node.js, Python, whatever)
node generate-document.js

# Convert to PDF - headless, no GUI window, no user interaction
soffice --headless --convert-to pdf document.odt --outdir /tmp/

# Render each page to PNG for visual inspection
pdftoppm -r 150 -png /tmp/document.pdf /tmp/page

# Now inspect: Read /tmp/page-01.png, /tmp/page-02.png, etc.
```

That's it. Generate, convert, render, inspect. All from the terminal. All scriptable. All automatable.

**Requirements:**
- `brew install --cask libreoffice` (gives you `soffice`)
- `brew install poppler` (gives you `pdftoppm`)
- If macOS blocks LibreOffice with "damaged app" warning: `xattr -cr /Applications/LibreOffice.app`

---

## The Ecosystem Problem

Let's talk about what actually happened:

**Microsoft:** "We'll make Office, charge you every month, and ensure the format changes every few years so old files break. Oh, and it'll cost more on Mac."

**Apple:** "We'll make Pages so beautiful nobody cares that it can't export consistently. Also your formatting will mysteriously change when you open it on a different Mac."

**Google:** "We'll make Docs free and cloud-first, then surprise: your complex formatting disappears when you download it. Also your spreadsheet formulas are mildly broken."

**OnlyOffice:** "We'll claim to support ODF but secretly convert everything to OOXML behind your back. Also no CLI. Also no style inheritance. But hey, it's free!"

**LibreOffice:** "We'll make something that just... works. ODF-native. Full CLI. You can save it, send it to anyone, open it anywhere, modify it, automate it, and it'll look the same. Also it's free."

One of these is not like the others.

---

## For Claude (And Humans)

When Claude needs to create or edit a document:

1. **Source format:** ODF (OpenDocument Format) - the actual open standard
2. **Rendering:** LibreOffice - ODF-native, no conversion losses
3. **QA:** Headless pipeline - `soffice --headless` + `pdftoppm` + visual inspection
4. **Compatibility:** Tested across Windows, Mac, Linux, Web, Mobile
5. **Reliability:** No surprise formatting collapses when switching devices
6. **The sauce:** The document is the sauce. Not locked in some app's cloud. Not dependent on a subscription. Not reliant on browser rendering quirks.

LibreOffice reads and writes:
- `.docx` (Microsoft Word, but consistently)
- `.xlsx` (Excel, without corruption)
- `.pptx` (PowerPoint, actually properly)
- `.odt` (OpenDocument - native, pristine)
- `.ods` (Spreadsheets, the right way)
- `.odp` (Presentations, without vendor lock-in)
- PDF, RTF, CSV, and a dozen others

You can **save in one format, open in another, and the document just works.**

---

## The Real Talk

**Microsoft's Closed System:** Office is designed to keep you paying. Each version breaks compatibility. Each device costs more. The .docx format is "open" in the way a car is "open" if you sign an NDA to work on it.

**Apple Pages is a Crime:** It's gorgeous. It's intuitive. It's also incompatible with itself across devices. Export to .docx? Good luck. Your margins change. Your fonts change. Your carefully-formatted table becomes spaghetti. It's a beautiful lie.

**Google Drive Formatting Roulette:** Create a beautiful document in Docs. Download it as .docx. Open it in Word. Weep. The formatting isn't lost - it's been reorganized according to Google's understanding of CSS, which apparently differs from everyone else's.

**OnlyOffice (Deprecated):** Looked promising. Supported ODF. Then you realize it converts everything to OOXML under the hood, has no headless mode, no style inheritance, and your paragraph backgrounds randomly stop being full-width. It's Microsoft Office cosplaying as open source.

**LibreOffice:** "Here's your document. It looks the same on your Mac, your Linux server, your Windows laptop, and your colleague's Linux server. Also here's a CLI so you never have to open the GUI. You're welcome."

---

## Claude's Perspective

From an AI perspective, LibreOffice is the obvious choice:

- **Headless-first:** `soffice --headless --convert-to pdf` means zero GUI dependency for document operations
- **Predictability:** No surprise rendering. No "it works on my machine but your machine sees something else."
- **ODF-native:** The format Claude generates (.odt) is LibreOffice's native format. No conversion. No losses.
- **Transferability:** Document created on a Windows machine, edited on a Mac, stored on a Linux server, shared with anyone anywhere - zero friction.
- **Standards compliance:** ODF is an OASIS standard. Not a corporation's interpretation of openness. Actual openness.
- **Automation:** Scripts, CLI, batch operations work the same everywhere. No mouse required.

When Claude creates a document, that document should *be* the document. Not a rendering of the document. Not a cloud-dependent version. Not a "your version might differ" situation.

The document itself is the sauce. And LibreOffice respects that.

---

## Install

```bash
# macOS
brew install --cask libreoffice
brew install poppler  # for pdftoppm

# Fix Gatekeeper if it complains
xattr -cr /Applications/LibreOffice.app

# Linux
sudo apt install libreoffice poppler-utils

# Verify
soffice --version
pdftoppm -h
```

---

## Migration from OnlyOffice

If you're coming from OnlyOffice:

1. `brew uninstall --cask onlyoffice`
2. `brew install --cask libreoffice`
3. `xattr -cr /Applications/LibreOffice.app`
4. Remove OnlyOffice data: `rm -rf ~/Library/Application\ Support/asc.onlyoffice.ONLYOFFICE`
5. Update all documentation references from OnlyOffice to LibreOffice
6. Your .odt files work immediately - no conversion needed

---

*Made for Claude. Works for humans too.*
