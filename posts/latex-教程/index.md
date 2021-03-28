# LaTeX 教程


<!--more-->

源文件：`.tex`

生成文件：`.pdf`

## Preamble

```latex
% preamble start
\documentclass[12pt, letterpaper]{article}
\usepackage[utf8]{inputenc}

\title{LaTeX Tutorials}
\author{Backsided \thanks{funded by the Overleaf team}}
\date{December 2020}
% preamble end

\begin{document}
Hello World LaTeX!
\end{document}
```

```latex
\documentclass[12pt, letterpaper]{article}
```

- define the type of document (`article`).
  - `article` for short documents and journal articles, most commonly used.
  - `report` for longer documents and dissertations.
  - `book` for write books.
  - `letter` for letters.
  - `slides` for slides, rarely used.
  - `beamer` for slides in the Beamer class format.
- set the font size (`12pt`), the default size is `10pt`.
- set the paper size (`letterpaper`), others like: `A4`(default), `legalpaper`.
- `twocolumn` for two-column format.
- `twoside` for two-side paper sheet printing.

```latex
\usepackage[utf8]{inputenc}
```

- the encoding for the document.
- `utf8` is recommended.

## Title

```latex
\documentclass[12pt, letterpaper, twoside]{article}
\usepackage[utf8]{inputenc}

\title{LaTeX Tutorials}
\author{Backsided \thanks{funded by the Overleaf team}}
\date{December 2020}

\begin{document}

\begin{titlepage}
\maketitle
\end{titlepage}

Hello World LaTeX!

\end{document}
```

```latex
\title{LaTeX Tutorials}
```

- title

```latex
\author{Backsided \thanks{funded by the Overleaf team}}
```

- author
- (optional) `\thanks{}` for a superscript and a footnote.

```latex
\date{December 2020}
```

- date
- `\today` for updated automatically each time compiled.

```latex
\begin{titlepage}
\end{titlepage}
```

- individual first page

```latex
\maketitle
```

- print the title, the author and the date

## Abstract, paragraphs and newlines

```latex
\begin{abstract}
\end{abstract}
```

- abstract

```latex
\begin{document}
paragraph 1

paragraph \\ 2
\end{document}
```

- a blank line for new paragraph
- paragraphs indent before the first line
- `\\` or `\newline` for a new line

## Comments

```latex
\begin{document}
Hello World LaTeX!  % This is a single line comment.
\end{document}
```

```latex
\usepackage{comment}

\begin{comment}
This is a multi-line comment.
\end{comment}
```

## Chinese for Overleaf

https://cn.overleaf.com/learn/how-to/Changing_compiler

`Menu->Compiler->XeLaTeX`

### ctex

```latex
\documentclass{ctexart}  % 中文article

\begin{document}

\tableofcontents  % 目录

\begin{abstract}  % 摘要
这是简介及摘要。
\end{abstract}

\section{前言}  % 第一章

\section{关于数学部分}  % 第二章

数学、中英文皆可以混排。You can intersperse math, Chinese and English (Latin script) without adding extra environments.

這是繁體中文。

\end{document}
```

```latex
\documentclass{ctexart}
```

- 文章类型：`ctexart`、`ctexrep`、`ctexbook`、`ctexbeamer`

```latex
\setCJKmainfont{}  % 全局字体
\setCJKsansfont{}  % 英文字体
\setCJKmonofont{}  % 等宽字体
```

- [Chinese fonts available on Overleaf](https://cn.overleaf.com/learn/latex/Questions/Which%20OTF%20or%20TTF%20fonts%20are%20supported%20via%20fontspec%3F#Chinese)

```latex
\documentclass{UniThesis}
\usepackage{ctex}
```

- different document class still like to use the `ctex` bundle

### xeCJK with XeLaTeX

you just want to typeset some Chinese characters

```latex
\documentclass{article}
\usepackage{xeCJK}
\begin{document}

\section{前言}

\section{关于数学部分}
数学、中英文皆可以混排。You can intersperse math, Chinese and English (Latin script) without adding extra environments.

這是繁體中文。
\end{document}
```

- The `xeCJK` package only works when compiled with XeLaTeX.

### CJKutf8 with pdfLaTeX

only convenient for documents in English with bits of Chinese text or vice-versa

```latex
\documentclass{article}
\usepackage{CJKutf8}

\begin{document}

\begin{CJK*}{UTF8}{gbsn}

\section{前言}

\section{关于数学部分}
数学、中英文皆可以混排。You can intersperse math, Chinese and English (Latin script) without adding extra environments.

\end{CJK*}

\bigskip  %% Just some white space

You can also insert Latin text in your document

\bigskip  %% Just some white space

\begin{CJK*}{UTF8}{bsmi}
這是繁體中文。
\end{CJK*}

\end{document}
```

```latex
\usepackage{CJKutf8}
```

- enables utf8 encoding for Chinese, Japanese and Korean fonts.

```latex
\begin{CJK*}{UTF8}{gbsn}
\end{CJK*}
```

- `gbsn` or `gkai` fonts for simplified characters.
- `bsmi` or `bkai` fonts for traditional characters.
- [Using the CTeX Package on Overleaf (在Overleaf平台上使用CTeX)](https://www.overleaf.com/project/5fdc5824f829edba17051664)

## Algorithms

### Algorithm2e

```latex
\documentclass{article}
\usepackage[ruled,vlined]{algorithm2e}

\begin{document}
\begin{algorithm}[H]
\SetAlgoLined
\KwResult{Write here the result }
 initialization\;
 \While{While condition}{
  instructions\;
  \eIf{condition}{
   instructions1\;
   instructions2\;
   }{
   instructions3\;
  }
 }
 \caption{How to write algorithms}
\end{algorithm}
\end{document}
```

### Algorithmic

```latex
\documentclass{article}
\usepackage{algorithmic}

\begin{document}
\begin{algorithmic}
\STATE $i\gets 10$
\IF {$i\geq 5$}
        \STATE $i\gets i-1$
\ELSE
        \IF {$i\leq 3$}
                \STATE $i\gets i+2$
        \ENDIF
\ENDIF
\end{algorithmic}
\end{document}
```

### Listings

```latex
\documentclass{article}
\usepackage{listings}

\begin{document}

\lstset{numbers=left, numberstyle=\tiny, stepnumber=1, numbersep=5pt}

\lstinputlisting[language=c]{c.c}

\end{document}
```

```latex
\lstinputlisting[language=c]{c.c}
```

```latex
\lstset{language=Pascal}
```

- tiny line numbers on the left, each second line, with 5pt distance to the listing:

```latex
\lstset{numbers=left, numberstyle=\tiny, stepnumber=1, numbersep=5pt}
```

