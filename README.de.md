# LaTeX-Berichtsvorlage – HAW Kiel, M.Sc. Data Science

🇬🇧 [English version](README.md)

> **Inoffizielle Vorlage.** Sie wird nicht von der HAW Kiel herausgegeben oder
> geprüft. Maßgeblich sind immer die Prüfungsordnung und die Vorgaben eures
> Moduls bzw. Fachbereichs.

Modulare LaTeX-Vorlage für Projektberichte, Seminar- und Hausarbeiten,
Laborberichte und die Masterarbeit im Studiengang **Data Science (M.Sc.)** am
**Fachbereich Medien der Hochschule für Angewandte Wissenschaften Kiel**.

Erstellt von **Yaacoub Harkat**, basierend auf der Thesis-Vorlage der HAW Hamburg
von **Prof. Dr. Thomas Lehmann** und Mitwirkenden (siehe [Danksagung](#11-danksagung-und-herkunft)).
Prinzip: **Alle Einstellungen stehen in einer einzigen Datei
(`configuration/configuration.tex`)**, der Rest wird über Schalter gesteuert.

---

## Inhalt

1. [Ordnerstruktur](#1-ordnerstruktur)
2. [Einrichtung](#2-einrichtung)
3. [Schnellstart: neuen Bericht anlegen](#3-schnellstart-neuen-bericht-anlegen)
4. [Konfigurationsreferenz](#4-konfigurationsreferenz)
5. [Logo einbinden](#5-logo-einbinden)
6. [Inhalte schreiben](#6-inhalte-schreiben)
7. [Kompilieren](#7-kompilieren)
8. [Vorlage erweitern](#8-vorlage-erweitern)
9. [Häufige Probleme](#9-häufige-probleme)
10. [Unterschiede zur HAW-Hamburg-Vorlage](#10-unterschiede-zur-haw-hamburg-vorlage)
11. [Danksagung und Herkunft](#11-danksagung-und-herkunft)

---

## 1. Ordnerstruktur

```
report/
├── README.md                     ← Anleitung (Englisch)
├── README.de.md                  ← diese Anleitung (Deutsch)
├── style/                        ← gemeinsamer Stil für alle Berichte (normalerweise nicht anfassen)
│   ├── reportstyle.sty           ← Pakete, Layout, Kopf-/Fußzeilen, Code-Stil
│   ├── configurationcommands.sty ← alle Konfigurationsbefehle und ihre Logik
│   ├── convenience.sty           ← Hilfsbefehle (\includenamedimage, \DeEn, ...)
│   ├── pages/                    ← Bausteine: Deckblätter, Titelseite, Kurzfassung, Erklärung
│   │   ├── cover_hawkiel.tex
│   │   ├── cover_leitfaden.tex
│   │   ├── cover_simple.tex
│   │   ├── titlepage.tex
│   │   ├── abstractpage.tex
│   │   └── statement.tex
│   └── logo/                     ← hier das HAW-Kiel-Logo ablegen (siehe Abschnitt 5)
└── template/                     ← Kopiervorlage für einen Bericht
    ├── report.tex                ← Hauptdatei: bindet die Kapitel ein
    ├── configuration/
    │   └── configuration.tex     ← ALLE Einstellungen des Berichts
    ├── chapters/                 ← ein Kapitel pro Datei
    ├── appendix/                 ← Anhangskapitel
    ├── figures/                  ← Bilder
    ├── glossary_entries.tex      ← Abkürzungen, Glossar, Symbole
    ├── literature.bib            ← Literaturdatenbank (BibTeX)
    └── latexmkrc                 ← Build-Konfiguration für latexmk
```

**Prinzip:** `style/` existiert einmal und wird von allen Berichten genutzt.
Für jeden Bericht wird `template/` kopiert. Ändert man etwas am Stil, wirkt es
sich auf alle Berichte aus.

```
report/
├── style/
├── template/
├── projektbericht-application-project/   ← Kopie von template/
└── seminararbeit-deep-learning/          ← Kopie von template/
```

Die Berichtsordner müssen **auf derselben Ebene wie `style/`** liegen. Wer eine
andere Struktur möchte, passt in `report.tex` die Zeile
`\newcommand{\StyleDir}{../style}` an (siehe [Abschnitt 9](#9-häufige-probleme)).

---

## 2. Einrichtung

Empfohlen: **pdfLaTeX über latexmk**, Editor **VS Code mit LaTeX Workshop**.
Mit dieser Kombination ist die Vorlage getestet.

### 2.1 macOS (empfohlen)

**Schritt 1 – TeX-Distribution installieren** (MacTeX ohne GUI-Programme, ca. 5–6 GB;
der Installer fragt nach dem Mac-Passwort):

```bash
brew install --cask mactex-no-gui
```

Falls Homebrew dabei abbricht (z. B. weil eine Abhängigkeit aus dem Quellcode
gebaut werden muss – das passiert mit der Intel-Version von Homebrew auf
Apple-Silicon-Macs): MacTeX stattdessen direkt als `.pkg` von
[tug.org/mactex](https://tug.org/mactex/mactex-download.html) herunterladen und
per Doppelklick installieren.

Danach ein **neues** Terminal-Fenster öffnen (damit `/Library/TeX/texbin` im
`PATH` ist) und prüfen:

```bash
pdflatex --version && latexmk --version && makeindex --help 2>&1 | head -1
```

Optional: Pakete auf den neuesten Stand bringen:

```bash
sudo tlmgr update --self --all
```

**Schritt 2 – VS Code und LaTeX Workshop**

1. [VS Code](https://code.visualstudio.com/) installieren (falls noch nicht vorhanden).
   War VS Code während der MacTeX-Installation geöffnet: einmal komplett beenden
   (⌘Q) und neu starten, sonst findet es `latexmk` nicht.
2. Den Repo-Ordner `report/` in VS Code öffnen (*Datei → Ordner öffnen*).
3. VS Code schlägt die Erweiterung **LaTeX Workshop** (`James-Yu.latex-workshop`)
   vor (`.vscode/extensions.json`) → *Installieren*. Alternativ in der
   Erweiterungsansicht (⇧⌘X) nach „LaTeX Workshop“ suchen.
4. Optional: Befehl `code` im Terminal verfügbar machen – ⇧⌘P →
   *Shell Command: Install 'code' command in PATH*.

Die mitgelieferte `.vscode/settings.json` stellt LaTeX Workshop bereits passend
ein: Bauen mit latexmk beim Speichern, PDF-Vorschau als Tab in VS Code.

**Schritt 3 – erster Testlauf**

1. `template/report.tex` öffnen.
2. Speichern (⌘S) oder ⌥⌘B (*Build LaTeX project*).
3. Vorschau mit ⌥⌘V öffnen. ⌘-Klick ins PDF springt zur passenden Stelle im
   Quelltext (SyncTeX), ⌥⌘J vom Quelltext ins PDF.
4. Fehler und Warnungen erscheinen im Reiter *Probleme* (⇧⌘M); das vollständige
   Log über das TeX-Symbol links → *View LaTeX compiler log*.

Alternativ ohne Editor im Terminal:

```bash
cd template && latexmk report.tex && open report.pdf
```

### 2.2 Windows / Linux

- **Windows:** [TeX Live](https://tug.org/texlive/) (empfohlen) oder
  [MiKTeX](https://miktex.org/). Bei MiKTeX zusätzlich [Strawberry Perl](https://strawberryperl.com/)
  installieren – `latexmk` ist ein Perl-Skript.
- **Linux (Debian/Ubuntu):** `sudo apt install texlive-full latexmk`
  (schlanker: `texlive-latex-extra texlive-science texlive-fonts-extra texlive-lang-german latexmk`).
- Danach weiter wie in 2.1, Schritt 2.

### 2.3 Andere Editoren

- **TeXstudio:** *Optionen → TeXstudio konfigurieren → Erzeugen →
  Standardcompiler* auf `Latexmk` stellen.
- **Overleaf:** siehe [Abschnitt 7](#7-kompilieren).

---

## 3. Schnellstart: neuen Bericht anlegen

```bash
cp -R template projektbericht-xyz
```

```bash
cd projektbericht-xyz && latexmk report.tex
```

1. `template/` kopieren und sinnvoll benennen (siehe oben).
2. Einmal kompilieren – es sollte ohne Fehler eine `report.pdf` mit
   Beispielinhalten entstehen.
3. `configuration/configuration.tex` öffnen und ausfüllen: Berichtsart, Titel,
   Name, Matrikelnummer, Modul, Betreuung, Abgabedatum, Sprache …
4. Kapitel in `chapters/` schreiben, Literatur in `literature.bib` eintragen.
5. Beispielkapitel entfernen: Zeile `\input{chapters/99_beispiele}` in
   `report.tex` löschen (und die Beispieleinträge in `literature.bib` /
   `glossary_entries.tex`).
6. Vor der Abgabe: `\Draft{no}` setzen, Logo prüfen, Eigenständigkeitserklärung
   mit der Prüfungsordnung abgleichen.

---

## 4. Konfigurationsreferenz

Alle Befehle gehören in `configuration/configuration.tex`.

- Jeder Befehl ist **optional** – fehlt er, gilt der Standardwert.
- Die **Reihenfolge ist egal** (die Konfiguration wird erst bei `\begin{document}` ausgewertet).
- **Leere Textfelder** (`{}`) werden im Dokument einfach weggelassen.
- **Ungültige Werte** (z. B. `\Cover{blau}`) führen zu einer klaren Fehlermeldung.

### 4.1 Sprache und Art des Berichts

| Befehl | Werte | Standard | Wirkung |
|---|---|---|---|
| `\documentLanguage{…}` | `de`, `en` | `de` | Silbentrennung, automatische Überschriften („Inhaltsverzeichnis“ / „Contents“), Texte auf Deckblatt, Titelseite und Erklärung, Dezimaltrennzeichen bei `siunitx` |
| `\reportKind{…}` | `project`, `seminar`, `term`, `lab`, `internship`, `master` | `project` | Bezeichnung der Arbeit (siehe Tabelle unten) |
| `\reportKindCustom{DE}{EN}` | beliebiger Text | – | eigene Bezeichnung, z. B. `\reportKindCustom{Fallstudie}{Case Study}` |

| Schlüssel | Deutsch | Englisch |
|---|---|---|
| `project` | Projektbericht | Project Report |
| `seminar` | Seminararbeit | Seminar Paper |
| `term` | Hausarbeit | Term Paper |
| `lab` | Laborbericht | Lab Report |
| `internship` | Praktikumsbericht | Internship Report |
| `master` | Masterarbeit | Master's Thesis |

Bei `master` lautet die Titelseite „eingereicht zur Erlangung des akademischen
Grades Master of Science (M.Sc.)“ und die Betreuenden heißen
„Erstprüfer:in“ / „Zweitprüfer:in“.

### 4.2 Textfelder

| Befehl | Beispiel | Erscheint auf |
|---|---|---|
| `\reportTitle{…}` | `{Anomalieerkennung in\\ Sensordaten}` | Deckblatt, Titelseite, PDF-Metadaten, Erklärung |
| `\reportSubtitle{…}` | `{Ein Vergleich von Autoencodern}` | Deckblatt, Titelseite |
| `\reportTitleDE{…}` / `\reportTitleEN{…}` | Titel ohne `\\` | Kurzfassung bzw. Abstract (leer = `\reportTitle`) |
| `\reportAuthor{…}` | `{Erika Muster}` oder `{Erika Muster \\ Max Mustermann}` | überall |
| `\matriculationNumber{…}` | `{123456}` | Deckblatt |
| `\authorEmail{…}` | `{erika.muster@student.haw-kiel.de}` | Deckblatt (als Link) |
| `\authorAddress{…}` | `{Musterstraße 1, 24149 Kiel}` | nur Deckblatt `Leitfaden` (optional) |
| `\moduleName{…}` | `{Application Project}` | Deckblatt, Titelseite |
| `\semester{…}` | `{Sommersemester 2027}` | Deckblatt |
| `\semesterOfStudy{…}` (Alias `\fachsemester{…}`) | `{2}` | Deckblatt („Fachsemester“) |
| `\firstSupervisor[Bezeichnung]{…}` | `{Prof. Dr. Vorname Nachname}` | Deckblatt, Titelseite |
| `\secondSupervisor[Bezeichnung]{…}` | `{…}` | Deckblatt, Titelseite |
| `\cooperationPartner{…}` | `{Beispiel GmbH, Kiel}` | Deckblatt, Titelseite |
| `\submissionDate{…}` | `{15.03.2027}` (Standard: `\today`) | Deckblatt, Titelseite, Erklärung |
| `\submissionPlace{…}` | `{Kiel}` | Erklärung |
| `\keywordsDE{…}` / `\keywordsEN{…}` | `{Deep Learning, Zeitreihen}` | Kurzfassung, PDF-Metadaten |
| `\abstractDE{…}` / `\abstractEN{…}` | Absatztext | Kurzfassung / Abstract |
| `\aiUsage{…}` | `{ChatGPT (Sprachkorrektur), GitHub Copilot (Code)}` | Erklärung (nur mit `\AIStatement{yes}`) |
| `\university[Kurzname]{DE}{EN}` | – | Name der Hochschule (voreingestellt: HAW Kiel) |

Das optionale Argument der Betreuenden ersetzt die automatische Bezeichnung,
z. B. `\firstSupervisor[Dozent]{Prof. Dr. …}`.

### 4.3 Studiengang

| Befehl | Wirkung |
|---|---|
| `\studycourse{DS}` | Data Science, Master of Science (M.Sc.), Fachbereich Medien / Faculty of Media (Standard) |
| `\studycourseCustom{Studiengang}{Study programme}{Abschluss}{Abk.}{Fachbereich}{Faculty}` | beliebiger anderer Studiengang |

Beispiel: `\studycourseCustom{Informatik}{Computer Science}{Master of Science}{M.Sc.}{Fachbereich Informatik und Elektrotechnik}{Faculty of Computer Science and Electrical Engineering}`

### 4.4 Schalter (`yes` / `no`)

| Befehl | Standard | Wirkung |
|---|---|---|
| `\NDA{…}` | `no` | Sperrvermerk „STRENG VERTRAULICH“ / „STRICTLY CONFIDENTIAL“ auf dem Deckblatt |
| `\TitlePage{…}` | `no` | zusätzliche innere Titelseite (empfohlen für die Masterarbeit) |
| `\AbstractPage{…}` | `yes` | Seite mit Kurzfassung/Abstract |
| `\TableOfContents{…}` | `yes` | Inhaltsverzeichnis |
| `\ListOfFigures{…}` | `yes` | Abbildungsverzeichnis |
| `\ListOfTables{…}` | `yes` | Tabellenverzeichnis |
| `\ListOfListings{…}` | `no` | Quellcodeverzeichnis |
| `\ListOfAcronyms{…}` | `yes` | Abkürzungsverzeichnis |
| `\ListOfSymbols{…}` | `no` | Symbolverzeichnis |
| `\Glossary{…}` | `no` | Glossar am Ende |
| `\Bibliography{…}` | `yes` | Literaturverzeichnis (ohne Zitate auf `no` setzen, sonst meldet BibTeX einen Fehler) |
| `\Statement{…}` | `yes` | Eigenständigkeitserklärung am Ende |
| `\AIStatement{…}` | `no` | Absatz zur Nutzung generativer KI in der Erklärung (Text aus `\aiUsage`; leer = „keine KI genutzt“) |
| `\Draft{…}` | `no` | Entwurfsmodus: `\todo{…}`-Notizen sichtbar, „Entwurf – Datum“ in der Fußzeile. Bei `no` werden alle Todo-Notizen ausgeblendet |
| `\Logo{…}` | `yes` | Logo auf dem Deckblatt |

### 4.5 Gestaltung und Literatur

| Befehl | Werte | Standard | Wirkung |
|---|---|---|---|
| `\Cover{…}` | `HAWKiel`, `Leitfaden`, `Simple`, `none` | `HAWKiel` | Deckblatt: modern mit Akzentbalken / nach der Formatvorlage des Fachbereichs Medien / klassisch zentriert / keins |
| `\pageMargins{l}{r}{o}{u}` | Längen | – (KOMA-Satzspiegel) | exakte Seitenränder links/rechts/oben/unten, z. B. `{3cm}{2cm}{2.5cm}{2.5cm}`. Ohne den Befehl gilt `DIV`/`BCOR` aus `report.tex` |
| `\logoFile{…}` | Pfad ohne Endung | `\StyleDir/logo/haw-kiel-logo` | Logo-Datei (`.pdf`, `.png` oder `.jpg`) |
| `\logoHeight{…}` | Länge | `18mm` | Höhe des Logos |
| `\accentColor{…}` | HTML-Farbcode ohne `#` | `004B87` | Farbe für Linien auf dem Deckblatt und Links. **Platzhalter** – bei Bedarf an das Corporate Design der HAW Kiel anpassen |
| `\fontStyle{…}` | `serif`, `sans` | `serif` | Fließtext mit bzw. ohne Serifen |
| `\lineSpacing{…}` | `single`, `onehalf`, `double` | `onehalf` | Zeilenabstand |
| `\citationStyle{…}` | `numeric`, `authoryear` | `numeric` | `[1]` (Reihenfolge des Zitierens) bzw. „Hastie et al. (2009)“ |
| `\bibliographyFile{…}` | Dateiname ohne `.bib` | `literature` | BibTeX-Datei |

### 4.6 Vorgaben des Fachbereichs Medien

Der [Leitfaden für Wissenschaftliche Arbeiten](https://www.haw-kiel.de/fileadmin/data/multimedia/pruefungsangelegenheiten/projekte_praktika_thesen/leifaden_wissent_arbeiten_oeuu_ak.pdf)
des Fachbereichs Medien (Stand 19.01.2026, formal für die Studiengänge ÖuU und
AK) verlangt u. a. einseitigen Druck, Ränder 3 cm links / 2 cm rechts /
2,5 cm oben und unten, 12 pt, 1,5-zeilig und ein Deckblatt nach Formatvorlage.
Ob das für euren Studiengang bzw. euer Modul gilt, bitte beim Prüfungsamt oder
der betreuenden Person klären. So stellt man die Vorlage darauf ein:

```latex
% report.tex
\documentclass[a4paper, fontsize=12pt, oneside, ...]{scrreprt}

% configuration.tex
\pageMargins{3cm}{2cm}{2.5cm}{2.5cm}
\lineSpacing{onehalf}
\Cover{Leitfaden}
\semesterOfStudy{2}
```

### 4.7 Einstellungen in `report.tex`

Einige Dinge stehen bewusst in der Hauptdatei, weil sie die Dokumentklasse
betreffen:

- **Klassenoptionen** (`\documentclass[...]{scrreprt}`): Schriftgröße
  (`fontsize=11pt`/`12pt`), Satzspiegel (`DIV=10` – größere Zahl = schmalere
  Ränder), Bindekorrektur (`BCOR`), `oneside`/`twoside`.
- **Kapitelreihenfolge** über `\input{chapters/...}`.
- **Anhang**: `\input{appendix/a_anhang}` hinter `\appendix` einkommentieren.
- **Zusätzliche Pakete** nur für diesen Bericht: an der markierten Stelle vor
  `\begin{document}`.
- Die Reihenfolge der Vorspann-Bausteine (`\ICoverPage`, `\IAbstractPage`, …)
  kann verändert werden – z. B. die Erklärung `\IStatementPage` nach vorne
  holen, falls das Modul das verlangt.

---

## 5. Logo einbinden

Das offizielle Logo ist **nicht** in der Vorlage enthalten (Markenrecht).
Solange keine Logo-Datei existiert, erscheint auf dem Deckblatt ein
Platzhalter-Kasten und das Log enthält eine Warnung – das Dokument baut
trotzdem.

1. Logo der HAW Kiel besorgen (Corporate-Design-/Presse-Downloads der
   Hochschule), idealerweise als Vektor-PDF.
2. Als `style/logo/haw-kiel-logo.pdf` speichern (alternativ `.png`/`.jpg`).
3. Neu kompilieren. Größe ggf. mit `\logoHeight{…}` anpassen.

Ohne Logo: `\Logo{no}`. Anderer Pfad: `\logoFile{…}`.

---

## 6. Inhalte schreiben

Das Kapitel `chapters/99_beispiele.tex` zeigt alles Folgende als
kompilierbares Beispiel.

### Kapitel
Eine Datei pro Kapitel in `chapters/`, eingebunden in `report.tex`. Die
Überschriften der Vorlage nutzen `\DeEn{Deutsch}{English}`, damit sie beim
Sprachwechsel mitwechseln – für eigene Kapitel reicht ganz normaler Text:
`\chapter{Einleitung}`.

### Abbildungen
Bilder in `figures/` ablegen. Kurzform mit automatischem Label `fig:<Datei>`:

```latex
\includenamedimage[0.7]{architektur}{Architektur des Systems}
% Verweis: \autoref{fig:architektur}
```

Diagramme lassen sich auch direkt mit `pgfplots` zeichnen (siehe Beispielkapitel).
Für Plots aus Python (`matplotlib`) empfiehlt sich der Export als PDF:
`plt.savefig("figures/plot.pdf", bbox_inches="tight")`.

### Tabellen
Mit `booktabs` (`\toprule`, `\midrule`, `\bottomrule`) und für Zahlen die
Spalte `S` aus `siunitx` (Ausrichtung am Dezimaltrennzeichen).

### Quellcode
```latex
\begin{lstlisting}[language=Python, caption={Training}, label={lst:train}]
model.fit(X_train, y_train)
\end{lstlisting}
```
Standardsprache ist Python; mit `language=SQL`, `language=R`, … umstellen.
Code im Fließtext: `\lstinline|df.groupby("x")|`. Das Quellcodeverzeichnis
schaltet `\ListOfListings{yes}` ein.

### Zitieren
Einträge in `literature.bib`, im Text `\cite{key}`, `\citet{key}` (Autor im
Text) oder `\cite[S.~5]{key}`. Stil über `\citationStyle`. Bei
`authoryear` für Klammerzitate `\citep{key}` → „(Hastie et al., 2009)“ und
`\citet{key}` → „Hastie et al. (2009)“ verwenden.

### Abkürzungen, Glossar, Symbole
Einträge in `glossary_entries.tex`, im Text `\gls{label}`. In den
Verzeichnissen erscheinen nur Einträge, die im Text verwendet werden.

### Notizen
`\todo{…}` setzt Randnotizen (nur mit `\Draft{yes}` sichtbar).

---

## 7. Kompilieren

### Empfohlen: latexmk
Im Berichtsordner:

```bash
latexmk report.tex
```

`latexmk` liest die mitgelieferte `latexmkrc` und führt automatisch pdflatex,
BibTeX und makeindex (für Abkürzungen/Glossar/Symbole) so oft wie nötig aus.

Aufräumen der Hilfsdateien:

```bash
latexmk -c
```

### Manuell
```bash
pdflatex report && bibtex report && makeglossaries report && pdflatex report && pdflatex report
```

### Editoren
- **VS Code + LaTeX Workshop:** nutzt standardmäßig latexmk – `report.tex`
  öffnen und speichern genügt.
- **TeXstudio:** *Optionen → TeXstudio konfigurieren → Erzeugen →
  Standardcompiler* auf `Latexmk` stellen. `report.tex` als Root-Dokument.
- **Overleaf:** den gesamten Ordner (inkl. `style/`) als ZIP hochladen und
  `template/report.tex` als Hauptdokument wählen. Falls der Style-Ordner nicht
  gefunden wird: `style/` in den Berichtsordner verschieben und
  `\newcommand{\StyleDir}{style}` setzen; die `latexmkrc` gehört dann ins
  Projektwurzelverzeichnis.

---

## 8. Vorlage erweitern

Alle Erweiterungen erfolgen in `style/` und stehen danach allen Berichten zur
Verfügung.

### Neue Berichtsart
In `style/configurationcommands.sty`, Abschnitt 6, eine Zeile ergänzen:

```latex
\IkindCase{poster}{Posterbeitrag}{Poster Contribution}%
```

Danach ist `\reportKind{poster}` nutzbar. Für einmalige Fälle reicht
`\reportKindCustom{…}{…}` in der Konfiguration.

### Neuer Studiengang (Preset)
Im selben Abschnitt nach dem Block für `DS` einen weiteren Block einfügen:

```latex
\ifthenelse{\equal{\Istudycourse}{XY}}{%
  \renewcommand{\IcourseDE}{...}\renewcommand{\IcourseEN}{...}%
  \renewcommand{\IdegreeName}{Master of Science}\renewcommand{\IdegreeShort}{M.Sc.}%
  \renewcommand{\IfacultyDE}{Fachbereich ...}\renewcommand{\IfacultyEN}{Faculty of ...}%
  \Ifoundtrue}{}%
```

### Neues Deckblatt
1. `style/pages/cover_<name>.tex` anlegen (am einfachsten `cover_simple.tex`
   kopieren). Verfügbar sind u. a. `\IreportTitle`, `\IreportSubtitle`,
   `\IreportAuthor`, `\IreportKindName`, `\Iuniversity`, `\Ifaculty`,
   `\ILogo`, `\IInfoBlock`, `\IAuthorDetails`, `\INDAText` sowie die Schalter
   `\ifINDA`, `\ifILogo`, `\ifIEnglish`.
2. In `configurationcommands.sty` in `\ICoverPage` (Abschnitt 7) eine Zeile ergänzen:
   `\ifthenelse{\equal{\ICover}{<Name>}}{\input{\StyleDir/pages/cover_<name>}}{}%`
3. Den Namen unter „Check cover“ (Abschnitt 6) ergänzen.

### Neuer Schalter
```latex
\IdefineSwitch{MeinSchalter}{no}   % in configurationcommands.sty, Abschnitt 3
```
Danach gibt es `\MeinSchalter{yes|no}` für die Konfiguration und
`\ifIMeinSchalter … \fi` für die Abfrage.

### Neues Textfeld
```latex
\IdefineField{meinFeld}{Standardwert}   % in configurationcommands.sty, Abschnitt 2
```
Konfiguration: `\meinFeld{…}`; Verwendung: `\ImeinFeld`, leer-Test mit
`\ifdefempty{\ImeinFeld}{leer}{nicht leer}`.

---

## 9. Häufige Probleme

| Problem | Lösung |
|---|---|
| `File 'reportstyle.sty' not found` | Der Berichtsordner liegt nicht neben `style/`. Ordner verschieben oder `\StyleDir` in `report.tex` anpassen. |
| Abkürzungs-/Symbolverzeichnis fehlt | `makeglossaries` bzw. makeindex wurde nicht ausgeführt → mit `latexmk` bauen. Es erscheinen nur Einträge, die mit `\gls{…}` verwendet werden. |
| BibTeX-Fehler „I found no \citation commands“ | Noch nichts zitiert → `\Bibliography{no}` oder etwas zitieren. |
| Literaturverzeichnis/Verweise zeigen `?` | Noch einmal kompilieren bzw. latexmk verwenden. |
| Warnung „Logo file … not found“ | Logo nach `style/logo/haw-kiel-logo.pdf` legen (Abschnitt 5) oder `\Logo{no}`. |
| `Invalid value '…' for the command …` | Tippfehler in `configuration.tex` – erlaubte Werte siehe Abschnitt 4. |
| Todo-Notizen fehlen | `\Draft{yes}` setzen. |
| Langer Titel bricht unschön um | Manuelle Umbrüche mit `\\` in `\reportTitle{…}` setzen; für Kurzfassung/Abstract `\reportTitleDE`/`\reportTitleEN` ohne Umbrüche angeben. |

**Wichtig:** Der Text der Eigenständigkeitserklärung (`style/pages/statement.tex`)
ist allgemein formuliert. Vor der Abgabe mit der aktuellen Prüfungsordnung
bzw. den Vorgaben des Moduls abgleichen und ggf. anpassen.

---

## 10. Unterschiede zur HAW-Hamburg-Vorlage

| HAW Hamburg (alt) | Diese Vorlage |
|---|---|
| Fester Inhalt: Thesis | Berichtsarten wählbar (`\reportKind`) |
| Nur Deutsch (Englisch „Beta“) | `\documentLanguage{en}` schaltet vollständig auf Englisch um |
| Studiengänge der Fakultät TI | Preset `DS` + `\studycourseCustom` für beliebige |
| Fehlender Konfigurationsbefehl → Fehler „undefined control sequence“ | Jeder Befehl hat einen Standardwert |
| Reihenfolge in der Konfiguration teils relevant | Reihenfolge egal |
| Logo fest eingebunden | Logo konfigurierbar, Platzhalter falls nicht vorhanden |
| Quellcodeverzeichnis hart in `thesis.tex` | Schalter `\ListOfListings` |
| Symbolverzeichnis ohne latexmk-Regel | latexmk-Regeln für Abkürzungen, Glossar **und** Symbole |
| Feste Ränder über `geometry` | KOMA-Satzspiegel oder exakte Ränder per `\pageMargins` |
| — | Neu: `\Draft`, `\AIStatement`, `\citationStyle`, `\fontStyle`, `\lineSpacing`, `\accentColor`, `\cooperationPartner`, `\matriculationNumber`, `\semesterOfStudy`, `\Cover{Leitfaden}` |

Weitergeführt wurden u. a. die Struktur `style/` + Berichtsordner, die
`\I…`-Namenskonvention, `\includenamedimage`, der Python-Code-Stil sowie
KOMA-Script (`scrreprt`) mit den bisherigen Klassenoptionen.

---

## 11. Danksagung und Herkunft

Diese Vorlage wurde von **Yaacoub Harkat** erstellt.

Sie basiert auf der **LaTeX-Thesis-Vorlage der HAW Hamburg** (Fakultät Technik
und Informatik), gepflegt von **Prof. Dr. Thomas Lehmann**, mit Beiträgen von
Klaus Jünemann, Edgar Toll, Jannik Beyerstedt, Lasse Lüder, Jonathan Ströbele,
Nima Ahmady-Moghaddam, Lukas Hettwer, Franziska Schult, Kim Henrik Otte,
Henning Krause und Johanna Pfeifer.

Übernommen bzw. weiterentwickelt wurden insbesondere das Konzept einer zentralen
Konfigurationsdatei mit Schaltern, die Aufteilung in einen gemeinsamen
`style/`-Ordner und Berichtsordner, die `\I…`-Namenskonvention, die Hilfsbefehle
in `convenience.sty` sowie die latexmk-Regeln für das Glossar. Vielen Dank an
alle, die an der ursprünglichen Vorlage mitgewirkt haben.

Die Formvorgaben für das Deckblatt `Leitfaden` stammen aus dem
[Leitfaden für Wissenschaftliche Arbeiten des Fachbereichs Medien der HAW Kiel](https://www.haw-kiel.de/fileadmin/data/multimedia/pruefungsangelegenheiten/projekte_praktika_thesen/leifaden_wissent_arbeiten_oeuu_ak.pdf).

Das Logo der HAW Kiel ist nicht enthalten; es ist markenrechtlich geschützt.
