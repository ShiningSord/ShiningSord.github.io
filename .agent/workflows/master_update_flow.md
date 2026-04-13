---
description: Master workflow for adding new items (publications, awards, etc.) to the website and CV
---

# Master Update Workflow

When the user requests to add a new item (e.g., a new publication, award, or experience), you MUST execute this complete end-to-end workflow to ensure the homepage and CV stay perfectly synchronized.

## Step 1: Update the Website Homepage (`index.jemdoc`)
1. Insert the new item into the appropriate section in `/Users/zxwang/Documents/ShiningSord.github.io/index.jemdoc`.
2. For any papers or PDFs, use **relative paths** pointing to the `papers/` directory (e.g., `[papers/C15-NewPaper.pdf paper]`).

## Step 2: Synchronize the LaTeX CV (`cv_source/docs/*.tex`)
1. Replicate the identically formatted information into the appropriate `.tex` file inside `cv_source/docs/` (e.g., `pub.tex` for publications).
2. **CRITICAL:** Convert all relative PDF links (from Step 1) into **absolute GitHub Pages links** (e.g., `\href{https://shiningsord.github.io/papers/C15-NewPaper.pdf}{[paper]}`). Do not use raw GitHub blob URLs.

## Step 3: Compile the Website
1. Compile `index.jemdoc` to HTML using the Python 2 conda environment:
   ```bash
   // turbo
   source ~/.zshrc && conda activate homepage && python jemdoc.py index.jemdoc
   ```

## Step 4: Compile the CV PDF
1. Ensure the new `.tex` modifications compile successfully to a PDF:
   ```bash
   // turbo
   cd cv_source && /Library/TeX/texbin/xelatex main.tex
   ```
2. Move the freshly generated `main.pdf` to replace the public `cv.pdf` at the root of the repository:
   ```bash
   // turbo
   mv cv_source/main.pdf cv.pdf
   ```

## Step 5: Git Version Control
1. Verify `git status` to ensure NO `.tex` source files or auxiliary LaTeX files are being tracked (they should be ignored by the `cv_source/` entry in `.gitignore`).
2. Stage and commit the changes using the **strict `YYYYMMDD` format**:
   ```bash
   // turbo
   git add index.jemdoc index.html cv.pdf
   // turbo
   git commit -m "$(date +%Y%m%d)"
   // turbo
   git push
   ```
   *(Note: Auto-run these steps only if the user has requested a full auto-update).*
