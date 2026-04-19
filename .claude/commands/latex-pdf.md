# LaTeX PDF Generator

Generate a LaTeX document, compile it to PDF, and fix any errors automatically.

## Instructions

The user's request: $ARGUMENTS

### Step 1 — Write the .tex file

Based on the request, write a well-formed `.tex` file. Apply these defaults unless the user specifies otherwise:
- Document class: `article`, 11pt font
- Margins: 1-inch via `\usepackage[margin=1in]{geometry}`
- Math: `\usepackage{amsmath, amssymb}`
- For econometric equations: use `align` environments, `\mathrm{}` for estimator labels

If no output path is specified, save to `pdfs/<descriptive-name>.tex`.

### Step 2 — Check for LaTeX availability

```bash
which pdflatex || which xelatex || which lualatex
```

If none are found, install via:
```bash
apt-get install -y texlive-full   # or texlive-latex-extra for a lighter install
```

### Step 3 — Compile

- For simple documents (no bibliography, no cross-references): use `pdflatex`
  ```bash
  pdflatex -interaction=nonstopmode -output-directory=pdfs pdfs/<file>.tex
  ```
- For documents with `\bibliography`, `\tableofcontents`, or `\ref`: use `latexmk`
  ```bash
  latexmk -pdf -interaction=nonstopmode -outdir=pdfs pdfs/<file>.tex
  ```

Ensure the output directory exists before compiling:
```bash
mkdir -p pdfs
```

### Step 4 — Read the log and fix errors

Read the `.log` file. Look for lines starting with `!` (errors) and `Warning` (potential issues). Common fixes:
- `File '...sty' not found` → install the missing package: `tlmgr install <package>` or `apt-get install texlive-<collection>`
- `Undefined control sequence` → check for typos in command names or missing `\usepackage`
- `Missing $ inserted` → math mode delimiter mismatch
- `Overfull \hbox` → cosmetic warning, safe to ignore unless the user cares about layout

Re-compile after each fix. Repeat until the log contains no `!` errors.

### Step 5 — Confirm output

```bash
ls -lh pdfs/<file>.pdf
```

Report the output path to the user and note any remaining warnings worth flagging.

## Examples of econometric LaTeX

Use these as reference for common estimators:

**OLS:**
```latex
\hat{\beta}_{\mathrm{OLS}} = (X^\top X)^{-1} X^\top y
```

**2SLS / IV:**
```latex
\hat{\beta}_{\mathrm{2SLS}} = \bigl(X^\top P_Z X\bigr)^{-1} X^\top P_Z y,
\qquad P_Z = Z(Z^\top Z)^{-1}Z^\top
```

**Difference-in-Differences (two-way FE):**
```latex
Y_{it} = \alpha + \tau D_{it} + \gamma_i + \delta_t + \varepsilon_{it}
```

**Sharp Regression Discontinuity:**
```latex
\tau_{\mathrm{RD}}
  = \lim_{x \downarrow c} \mathbb{E}[Y \mid X = x]
  - \lim_{x \uparrow   c} \mathbb{E}[Y \mid X = x]
```

**Panel Fixed Effects (within estimator):**
```latex
\hat{\beta}_{\mathrm{FE}}
  = (\ddot{X}^\top\ddot{X})^{-1}\ddot{X}^\top\ddot{y},
\quad \ddot{y}_{it} \equiv y_{it} - \bar{y}_i
```

**Efficient GMM:**
```latex
\hat{\theta}_{\mathrm{GMM}}
  = \arg\min_{\theta}\; g(\theta)^\top S^{-1} g(\theta)
```
