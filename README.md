# LaTeX Report Template – HAW Kiel, M.Sc. Data Science

🇩🇪 [Deutsche Version](README.de.md)

> **Unofficial template.** It is not published or reviewed by HAW Kiel. The
> examination regulations and the requirements of your module or faculty
> always take precedence.

A modular LaTeX template for project reports, seminar and term papers, lab
reports and the master's thesis in the **Data Science (M.Sc.)** programme at the
**Faculty of Media, Kiel University of Applied Sciences (HAW Kiel)**.

Created by **Yaacoub Harkat**, based on the HAW Hamburg thesis template by
**Prof. Dr. Thomas Lehmann** and contributors (see [Credits](#11-credits)).
Principle: **all settings live in a single file
(`configuration/configuration.tex`)**; everything else is controlled by switches.

---

## Contents

1. [Folder structure](#1-folder-structure)
2. [Setup](#2-setup)
3. [Quick start: create a new report](#3-quick-start-create-a-new-report)
4. [Configuration reference](#4-configuration-reference)
5. [Adding the logo](#5-adding-the-logo)
6. [Writing content](#6-writing-content)
7. [Compiling](#7-compiling)
8. [Extending the template](#8-extending-the-template)
9. [Troubleshooting](#9-troubleshooting)
10. [Differences from the HAW Hamburg template](#10-differences-from-the-haw-hamburg-template)
11. [Credits](#11-credits)

---

## 1. Folder structure

```
report/
├── README.md                     ← this guide (English)
├── README.de.md                  ← guide (German)
├── style/                        ← shared style for all reports (normally not touched)
│   ├── reportstyle.sty           ← packages, layout, headers/footers, code style
│   ├── configurationcommands.sty ← all configuration commands and their logic
│   ├── convenience.sty           ← helper commands (\includenamedimage, \DeEn, ...)
│   ├── pages/                    ← building blocks: cover pages, title page, abstract, declaration
│   │   ├── cover_hawkiel.tex
│   │   ├── cover_leitfaden.tex
│   │   ├── cover_simple.tex
│   │   ├── titlepage.tex
│   │   ├── abstractpage.tex
│   │   └── statement.tex
│   └── logo/                     ← put the HAW Kiel logo here (see section 5)
└── template/                     ← copy this folder for every report
    ├── report.tex                ← main file: includes the chapters
    ├── configuration/
    │   └── configuration.tex     ← ALL settings of the report
    ├── chapters/                 ← one file per chapter
    ├── appendix/                 ← appendix chapters
    ├── figures/                  ← images
    ├── glossary_entries.tex      ← abbreviations, glossary, symbols
    ├── literature.bib            ← bibliography database (BibTeX)
    └── latexmkrc                 ← build configuration for latexmk
```

**Principle:** `style/` exists once and is shared by all reports. For every
report you copy `template/`. Changes to the style affect all reports.

```
report/
├── style/
├── template/
├── project-report-application-project/   ← copy of template/
└── seminar-paper-deep-learning/          ← copy of template/
```

Report folders must be **on the same level as `style/`**. For a different
structure, change the line `\newcommand{\StyleDir}{../style}` in `report.tex`
(see [section 9](#9-troubleshooting)).

---

## 2. Setup

Recommended: **pdfLaTeX via latexmk**, editor **VS Code with LaTeX Workshop**.
The template is tested with this combination.

### 2.1 macOS (recommended)

**Step 1 – install a TeX distribution** (MacTeX without GUI apps, approx. 5–6 GB;
the installer asks for your password):

```bash
brew install --cask mactex-no-gui
```

If Homebrew fails (e.g. because a dependency has to be built from source – this
happens with the Intel version of Homebrew on Apple Silicon Macs), download
MacTeX as a `.pkg` from [tug.org/mactex](https://tug.org/mactex/mactex-download.html)
and install it by double-clicking.

Then open a **new** terminal window (so that `/Library/TeX/texbin` is on the
`PATH`) and check:

```bash
pdflatex --version && latexmk --version && makeindex --help 2>&1 | head -1
```

Optional: update all packages:

```bash
sudo tlmgr update --self --all
```

**Step 2 – VS Code and LaTeX Workshop**

1. Install [VS Code](https://code.visualstudio.com/) (if not already installed).
   If VS Code was open during the MacTeX installation, quit it completely (⌘Q)
   and restart it, otherwise it will not find `latexmk`.
2. Open the repository folder `report/` in VS Code (*File → Open Folder*).
3. VS Code suggests the extension **LaTeX Workshop** (`James-Yu.latex-workshop`)
   (`.vscode/extensions.json`) → *Install*. Alternatively search for
   "LaTeX Workshop" in the extensions view (⇧⌘X).
4. Optional: make the `code` command available in the terminal – ⇧⌘P →
   *Shell Command: Install 'code' command in PATH*.

The included `.vscode/settings.json` already configures LaTeX Workshop: build
with latexmk on save, PDF preview as a tab in VS Code.

**Step 3 – first test run**

1. Open `template/report.tex`.
2. Save (⌘S) or press ⌥⌘B (*Build LaTeX project*).
3. Open the preview with ⌥⌘V. ⌘-click in the PDF jumps to the source (SyncTeX),
   ⌥⌘J jumps from the source to the PDF.
4. Errors and warnings appear in the *Problems* tab (⇧⌘M); the full log is
   available via the TeX icon on the left → *View LaTeX compiler log*.

Without an editor, in the terminal:

```bash
cd template && latexmk report.tex && open report.pdf
```

### 2.2 Windows / Linux

- **Windows:** [TeX Live](https://tug.org/texlive/) (recommended) or
  [MiKTeX](https://miktex.org/). With MiKTeX also install
  [Strawberry Perl](https://strawberryperl.com/) – `latexmk` is a Perl script.
- **Linux (Debian/Ubuntu):** `sudo apt install texlive-full latexmk`
  (smaller: `texlive-latex-extra texlive-science texlive-fonts-extra texlive-lang-german latexmk`).
- Then continue with 2.1, step 2.

### 2.3 Other editors

- **TeXstudio:** *Options → Configure TeXstudio → Build → Default Compiler*:
  `Latexmk`.
- **Overleaf:** see [section 7](#7-compiling).

---

## 3. Quick start: create a new report

```bash
cp -R template my-project-report
```

```bash
cd my-project-report && latexmk report.tex
```

1. Copy `template/` and give it a meaningful name (see above).
2. Compile once – a `report.pdf` with example content should be created
   without errors.
3. Open `configuration/configuration.tex` and fill it in: kind of report,
   title, name, matriculation number, module, supervisors, submission date,
   language …
4. Write your chapters in `chapters/`, add references to `literature.bib`.
5. Remove the example chapter: delete the line `\input{chapters/99_beispiele}`
   in `report.tex` (and the example entries in `literature.bib` /
   `glossary_entries.tex`).
6. Before submitting: set `\Draft{no}`, check the logo, compare the declaration
   of authorship with your examination regulations.

---

## 4. Configuration reference

All commands go into `configuration/configuration.tex`.

- Every command is **optional** – if it is missing, the default applies.
- The **order does not matter** (the configuration is evaluated at `\begin{document}`).
- **Empty text fields** (`{}`) are simply omitted from the document.
- **Invalid values** (e.g. `\Cover{blue}`) produce a clear error message.

### 4.1 Language and kind of report

| Command | Values | Default | Effect |
|---|---|---|---|
| `\documentLanguage{…}` | `de`, `en` | `de` | hyphenation, automatic headings ("Contents" / "Inhaltsverzeichnis"), texts on cover, title page and declaration, decimal separator in `siunitx` |
| `\reportKind{…}` | `project`, `seminar`, `term`, `lab`, `internship`, `master` | `project` | name of the work (see table below) |
| `\reportKindCustom{DE}{EN}` | any text | – | custom name, e.g. `\reportKindCustom{Fallstudie}{Case Study}` |

| Key | German | English |
|---|---|---|
| `project` | Projektbericht | Project Report |
| `seminar` | Seminararbeit | Seminar Paper |
| `term` | Hausarbeit | Term Paper |
| `lab` | Laborbericht | Lab Report |
| `internship` | Praktikumsbericht | Internship Report |
| `master` | Masterarbeit | Master's Thesis |

With `master`, the title page reads "submitted for the degree of Master of
Science (M.Sc.)" and the supervisors are labelled "First examiner" /
"Second examiner".

### 4.2 Text fields

| Command | Example | Appears on |
|---|---|---|
| `\reportTitle{…}` | `{Anomaly Detection in\\ Sensor Data}` | cover, title page, PDF metadata, declaration |
| `\reportSubtitle{…}` | `{A Comparison of Autoencoders}` | cover, title page |
| `\reportTitleDE{…}` / `\reportTitleEN{…}` | title without `\\` | German / English abstract (empty = `\reportTitle`) |
| `\reportAuthor{…}` | `{Erika Muster}` or `{Erika Muster \\ Max Mustermann}` | everywhere |
| `\matriculationNumber{…}` | `{123456}` | cover |
| `\authorEmail{…}` | `{erika.muster@student.haw-kiel.de}` | cover (as link) |
| `\authorAddress{…}` | `{Musterstraße 1, 24149 Kiel}` | cover `Leitfaden` only (optional) |
| `\moduleName{…}` | `{Application Project}` | cover, title page |
| `\semester{…}` | `{Summer term 2027}` | cover |
| `\semesterOfStudy{…}` (alias `\fachsemester{…}`) | `{2}` | cover ("semester of study") |
| `\firstSupervisor[label]{…}` | `{Prof. Dr. First Last}` | cover, title page |
| `\secondSupervisor[label]{…}` | `{…}` | cover, title page |
| `\cooperationPartner{…}` | `{Example GmbH, Kiel}` | cover, title page |
| `\submissionDate{…}` | `{15.03.2027}` (default: `\today`) | cover, title page, declaration |
| `\submissionPlace{…}` | `{Kiel}` | declaration |
| `\keywordsDE{…}` / `\keywordsEN{…}` | `{Deep Learning, Time Series}` | abstract, PDF metadata |
| `\abstractDE{…}` / `\abstractEN{…}` | paragraph text | German / English abstract |
| `\aiUsage{…}` | `{ChatGPT (language editing), GitHub Copilot (code)}` | declaration (only with `\AIStatement{yes}`) |
| `\university[short]{DE}{EN}` | – | name of the university (default: HAW Kiel) |

The optional argument of the supervisor commands replaces the automatic label,
e.g. `\firstSupervisor[Lecturer]{Prof. Dr. …}`.

### 4.3 Degree programme

| Command | Effect |
|---|---|
| `\studycourse{DS}` | Data Science, Master of Science (M.Sc.), Faculty of Media (default) |
| `\studycourseCustom{Programme DE}{Programme EN}{Degree}{Abbr.}{Faculty DE}{Faculty EN}` | any other programme |

Example: `\studycourseCustom{Informatik}{Computer Science}{Master of Science}{M.Sc.}{Fachbereich Informatik und Elektrotechnik}{Faculty of Computer Science and Electrical Engineering}`

### 4.4 Switches (`yes` / `no`)

| Command | Default | Effect |
|---|---|---|
| `\NDA{…}` | `no` | confidentiality notice "STRICTLY CONFIDENTIAL" / "STRENG VERTRAULICH" on the cover |
| `\TitlePage{…}` | `no` | additional inner title page (recommended for the master's thesis) |
| `\AbstractPage{…}` | `yes` | abstract page |
| `\TableOfContents{…}` | `yes` | table of contents |
| `\ListOfFigures{…}` | `yes` | list of figures |
| `\ListOfTables{…}` | `yes` | list of tables |
| `\ListOfListings{…}` | `no` | list of listings |
| `\ListOfAcronyms{…}` | `yes` | list of abbreviations |
| `\ListOfSymbols{…}` | `no` | list of symbols |
| `\Glossary{…}` | `no` | glossary at the end |
| `\Bibliography{…}` | `yes` | bibliography (set to `no` if nothing is cited, otherwise BibTeX reports an error) |
| `\Statement{…}` | `yes` | declaration of authorship at the end |
| `\AIStatement{…}` | `no` | paragraph on the use of generative AI in the declaration (text from `\aiUsage`; empty = "no AI used") |
| `\Draft{…}` | `no` | draft mode: `\todo{…}` notes visible, "Draft – date" in the footer. With `no`, all todo notes are hidden |
| `\Logo{…}` | `yes` | logo on the cover |

### 4.5 Layout and bibliography

| Command | Values | Default | Effect |
|---|---|---|---|
| `\Cover{…}` | `HAWKiel`, `Leitfaden`, `Simple`, `none` | `HAWKiel` | cover page: modern with accent bar / following the Faculty of Media title page template / classic centred / none |
| `\pageMargins{l}{r}{t}{b}` | lengths | – (KOMA type area) | exact page margins left/right/top/bottom, e.g. `{3cm}{2cm}{2.5cm}{2.5cm}`. Without this command, `DIV`/`BCOR` from `report.tex` apply |
| `\logoFile{…}` | path without extension | `\StyleDir/logo/haw-kiel-logo` | logo file (`.pdf`, `.png` or `.jpg`) |
| `\logoHeight{…}` | length | `18mm` | height of the logo |
| `\accentColor{…}` | HTML colour code without `#` | `004B87` | colour of lines on the cover and of links. **Placeholder** – adapt to the HAW Kiel corporate design if needed |
| `\fontStyle{…}` | `serif`, `sans` | `serif` | body text with or without serifs |
| `\lineSpacing{…}` | `single`, `onehalf`, `double` | `onehalf` | line spacing |
| `\citationStyle{…}` | `numeric`, `authoryear` | `numeric` | `[1]` (order of citation) or "Hastie et al. (2009)" |
| `\bibliographyFile{…}` | file name without `.bib` | `literature` | BibTeX file |

### 4.6 Faculty of Media requirements

The [guideline for academic work](https://www.haw-kiel.de/fileadmin/data/multimedia/pruefungsangelegenheiten/projekte_praktika_thesen/leifaden_wissent_arbeiten_oeuu_ak.pdf)
of the Faculty of Media (as of 19 Jan 2026, formally for the programmes ÖuU and
AK) requires, among other things, one-sided printing, margins of 3 cm left /
2 cm right / 2.5 cm top and bottom, 12 pt, 1.5 line spacing and a cover page
following its template. Please check with the examination office or your
supervisor whether this applies to your programme or module. To configure the
template accordingly:

```latex
% report.tex
\documentclass[a4paper, fontsize=12pt, oneside, ...]{scrreprt}

% configuration.tex
\pageMargins{3cm}{2cm}{2.5cm}{2.5cm}
\lineSpacing{onehalf}
\Cover{Leitfaden}
\semesterOfStudy{2}
```

### 4.7 Settings in `report.tex`

Some things deliberately live in the main file because they concern the
document class:

- **Class options** (`\documentclass[...]{scrreprt}`): font size
  (`fontsize=11pt`/`12pt`), type area (`DIV=10` – larger number = narrower
  margins), binding correction (`BCOR`), `oneside`/`twoside`.
- **Chapter order** via `\input{chapters/...}`.
- **Appendix**: uncomment `\input{appendix/a_anhang}` after `\appendix`.
- **Additional packages** for this report only: at the marked place before
  `\begin{document}`.
- The order of the front matter blocks (`\ICoverPage`, `\IAbstractPage`, …)
  can be changed – e.g. move the declaration `\IStatementPage` to the front if
  your module requires it.

---

## 5. Adding the logo

The official logo is **not** included in the template (trademark law). As long
as no logo file exists, the cover shows a placeholder box and the log contains
a warning – the document still builds.

1. Get the HAW Kiel logo (corporate design / press downloads of the
   university), ideally as a vector PDF.
2. Save it as `style/logo/haw-kiel-logo.pdf` (alternatively `.png`/`.jpg`).
   This path is listed in `.gitignore` so the logo is not published by accident.
3. Recompile. Adjust the size with `\logoHeight{…}` if needed.

No logo: `\Logo{no}`. Different path: `\logoFile{…}`.

---

## 6. Writing content

The chapter `chapters/99_beispiele.tex` shows everything below as a compilable
example.

### Chapters
One file per chapter in `chapters/`, included in `report.tex`. Every chapter
file starts with `% !TEX root = ../report.tex` so that the editor always builds
the whole document. The template headings use `\DeEn{German}{English}` so they
switch with the document language – for your own chapters plain text is
enough: `\chapter{Introduction}`.

### Figures
Put images into `figures/`. Short form with automatic label `fig:<file>`:

```latex
\includenamedimage[0.7]{architecture}{System architecture}
% reference: \autoref{fig:architecture}
```

Plots can also be drawn directly with `pgfplots` (see the example chapter).
For plots from Python (`matplotlib`), export as PDF:
`plt.savefig("figures/plot.pdf", bbox_inches="tight")`.

### Tables
Use `booktabs` (`\toprule`, `\midrule`, `\bottomrule`) and for numbers the
column type `S` from `siunitx` (aligned at the decimal separator).

### Source code
```latex
\begin{lstlisting}[language=Python, caption={Training}, label={lst:train}]
model.fit(X_train, y_train)
\end{lstlisting}
```
The default language is Python; switch with `language=SQL`, `language=R`, ….
Inline code: `\lstinline|df.groupby("x")|`. Enable the list of listings with
`\ListOfListings{yes}`.

### Citations
Entries go into `literature.bib`; in the text use `\cite{key}`, `\citet{key}`
(author in the text) or `\cite[p.~5]{key}`. Style via `\citationStyle`. With
`authoryear`, use `\citep{key}` → "(Hastie et al., 2009)" and `\citet{key}` →
"Hastie et al. (2009)".

### Abbreviations, glossary, symbols
Entries go into `glossary_entries.tex`; in the text use `\gls{label}`. Only
entries used in the text appear in the lists.

### Notes
`\todo{…}` adds margin notes (only visible with `\Draft{yes}`).

---

## 7. Compiling

### Recommended: latexmk
In the report folder:

```bash
latexmk report.tex
```

`latexmk` reads the included `latexmkrc` and automatically runs pdflatex,
BibTeX and makeindex (for abbreviations/glossary/symbols) as often as needed.

Remove auxiliary files:

```bash
latexmk -c
```

### Manually
```bash
pdflatex report && bibtex report && makeglossaries report && pdflatex report && pdflatex report
```

### Editors
- **VS Code + LaTeX Workshop:** uses latexmk (see `.vscode/settings.json`) –
  opening and saving `report.tex` is enough.
- **TeXstudio:** set *Options → Configure TeXstudio → Build → Default
  Compiler* to `Latexmk`. Use `report.tex` as root document.
- **Overleaf:** upload the whole folder (including `style/`) as a ZIP and
  select `template/report.tex` as the main document. If the style folder is not
  found: move `style/` into the report folder and set
  `\newcommand{\StyleDir}{style}`; the `latexmkrc` then belongs in the project
  root.

---

## 8. Extending the template

All extensions are made in `style/` and are then available to all reports.

### New kind of report
In `style/configurationcommands.sty`, section 6, add a line:

```latex
\IkindCase{poster}{Posterbeitrag}{Poster Contribution}%
```

Afterwards `\reportKind{poster}` can be used. For one-off cases,
`\reportKindCustom{…}{…}` in the configuration is enough.

### New degree programme (preset)
In the same section, add another block after the one for `DS`:

```latex
\ifthenelse{\equal{\Istudycourse}{XY}}{%
  \renewcommand{\IcourseDE}{...}\renewcommand{\IcourseEN}{...}%
  \renewcommand{\IdegreeName}{Master of Science}\renewcommand{\IdegreeShort}{M.Sc.}%
  \renewcommand{\IfacultyDE}{Fachbereich ...}\renewcommand{\IfacultyEN}{Faculty of ...}%
  \Ifoundtrue}{}%
```

### New cover page
1. Create `style/pages/cover_<name>.tex` (easiest: copy `cover_simple.tex`).
   Available are e.g. `\IreportTitle`, `\IreportSubtitle`, `\IreportAuthor`,
   `\IreportKindName`, `\Iuniversity`, `\IuniversityShort`, `\Ifaculty`,
   `\ILogo`, `\IInfoBlock`, `\IAuthorDetails`, `\INDAText` and the switches
   `\ifINDA`, `\ifILogo`, `\ifIEnglish`.
2. In `configurationcommands.sty`, add a line to `\ICoverPage` (section 7):
   `\ifthenelse{\equal{\ICover}{<Name>}}{\input{\StyleDir/pages/cover_<name>}}{}%`
3. Add the name under "Check cover" (section 6).

### New switch
```latex
\IdefineSwitch{MySwitch}{no}   % in configurationcommands.sty, section 3
```
This provides `\MySwitch{yes|no}` for the configuration and
`\ifIMySwitch … \fi` for queries.

### New text field
```latex
\IdefineField{myField}{default value}   % in configurationcommands.sty, section 2
```
Configuration: `\myField{…}`; usage: `\ImyField`; emptiness test with
`\ifdefempty{\ImyField}{empty}{not empty}`.

---

## 9. Troubleshooting

| Problem | Solution |
|---|---|
| `File 'reportstyle.sty' not found` | The report folder is not next to `style/`. Move the folder or adjust `\StyleDir` in `report.tex`. |
| List of abbreviations/symbols missing | `makeglossaries`/makeindex was not run → build with `latexmk`. Only entries used with `\gls{…}` appear. |
| BibTeX error "I found no \citation commands" | Nothing cited yet → `\Bibliography{no}` or cite something. |
| Bibliography/references show `?` | Compile again or use latexmk. |
| Warning "Logo file … not found" | Put the logo at `style/logo/haw-kiel-logo.pdf` (section 5) or set `\Logo{no}`. |
| `Invalid value '…' for the command …` | Typo in `configuration.tex` – see section 4 for allowed values. |
| Todo notes missing | Set `\Draft{yes}`. |
| Long title breaks badly | Add manual breaks with `\\` in `\reportTitle{…}`; give `\reportTitleDE`/`\reportTitleEN` without breaks for the abstract page. |

**Important:** The text of the declaration of authorship
(`style/pages/statement.tex`) is generic. Before submitting, compare it with
the current examination regulations or module requirements and adapt it if
necessary.

---

## 10. Differences from the HAW Hamburg template

| HAW Hamburg (original) | This template |
|---|---|
| Fixed content: thesis | Kinds of report selectable (`\reportKind`) |
| German only (English "beta") | `\documentLanguage{en}` switches completely to English |
| Programmes of the Faculty of Engineering and Computer Science | Preset `DS` + `\studycourseCustom` for any programme |
| Missing configuration command → "undefined control sequence" | Every command has a default value |
| Order in the configuration partly relevant | Order does not matter |
| Logo hard-coded | Logo configurable, placeholder if missing |
| List of listings hard-coded in `thesis.tex` | Switch `\ListOfListings` |
| No latexmk rule for the list of symbols | latexmk rules for abbreviations, glossary **and** symbols |
| Fixed margins via `geometry` | KOMA type area or exact margins via `\pageMargins` |
| — | New: `\Draft`, `\AIStatement`, `\citationStyle`, `\fontStyle`, `\lineSpacing`, `\accentColor`, `\cooperationPartner`, `\matriculationNumber`, `\semesterOfStudy`, `\Cover{Leitfaden}` |

---

## 11. Credits

This template was created by **Yaacoub Harkat**.

It is based on the **LaTeX thesis template of HAW Hamburg** (Faculty of
Engineering and Computer Science), maintained by **Prof. Dr. Thomas Lehmann**,
with contributions by Klaus Jünemann, Edgar Toll, Jannik Beyerstedt, Lasse
Lüder, Jonathan Ströbele, Nima Ahmady-Moghaddam, Lukas Hettwer, Franziska
Schult, Kim Henrik Otte, Henning Krause and Johanna Pfeifer.

In particular, the concept of a central configuration file with switches, the
split into a shared `style/` folder and report folders, the `\I…` naming
convention, the helper commands in `convenience.sty` and the latexmk rules for
the glossary were adopted and developed further. Many thanks to everyone who
contributed to the original template.

The requirements for the `Leitfaden` cover page are taken from the
[guideline for academic work of the Faculty of Media, HAW Kiel](https://www.haw-kiel.de/fileadmin/data/multimedia/pruefungsangelegenheiten/projekte_praktika_thesen/leifaden_wissent_arbeiten_oeuu_ak.pdf).

The HAW Kiel logo is not included; it is protected by trademark law.
