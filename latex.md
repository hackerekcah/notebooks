# Latex (`la tech`)

## [Support Chinese](https://www.overleaf.com/learn/latex/chinese)
* change the compiler to `xeLatex` in MikTex console
* use `\documentclass{ctexart}` as document class.

## [equation](https://www.latex-tutorial.com/tutorials/amsmath/)
* Multiple Line
```
\begin{align*}
  f(x) &= x^2\\
  g(x) &= \frac{1}{x}\\
\end{align*}
```
* One line equation
```
\begin{equation*}
  1 = 3 - 2
\end{equation*}
```

## install latex and editors on windows
* install MikTex on windows
* MikTex is a LaTeX distribution, including `tex, latex, pdftex, pdflatex`
  * install packages in MikTex, and then \usepackage{} in TexStudio
* install TexStudio on windows  
  ** TexStudio internally use latex from MikTex
  
## [TeX, LateX, PDFLaTeX](https://www.overleaf.com/learn/latex/Articles/The_TeX_family_tree:_LaTeX,_pdfTeX,_XeTeX,_LuaTeX_and_ConTeXt)
* TeX is the original typsetting system created by knuth
* LaTeX is a modern version that introduce concepts like `usepackage`, 'environment'
* LaTeX will generate intermediate files such as PostScript
* PDFLaTeX will process .tex files directly to pdf.

## submit LaTex files to arxiv
* LaTex is preferred over pdf, because it contains most infomations, will use by the arxiv when necessary
* submmit a zip contains  
  * `.tex` file, add `\pdfoutput=1` to the preamble of the tex file, to indicate this tex file should be processed by `pdflatex`
  * `.bbl` file, this is generated using the `.bib` file
  * `.bst` bibligraph style file
  * `.sty` style file of the conference
  * `figures/` subdirectory, containing the figures
* No other auxillary files should be included. such as `.aux`, `.log`.

## bibtex, biber, biblatex, natbib
* `bibtex` and `biber` are programs that process `.bib` files, also called `backend`
* `natbib` and `biblatex` are LaTeX `packages` that format the bibligraphy
* see examples [biblatex-examples](http://ctan.cs.uu.nl/macros/latex/contrib/biblatex/doc/examples/biblatex-examples.bib)
* [bibtex entries](https://www.andy-roberts.net/res/writing/latex/bibentries.pdf)
* [bibtex entries manual](http://bib-it.sourceforge.net/help/fieldsAndEntryTypes.php)
* [arxiv2bibtex](https://arxiv2bibtex.org/?q=1904.05204+&format=bibtex)
* [arxiv bibtex stackexchange](https://tex.stackexchange.com/questions/49757/what-should-an-entry-for-arxiv-entries-look-like-for-biblatex)
  * eprinttype={arxiv}
  * eprintclass={cs.SD}
  * eprint={xxxx.xxxx}
  
## Beamer, latex ppt
* [Beamer wiki](https://en.wikibooks.org/wiki/LaTeX/Presentations)
* example
```
\documentclass{beamer}

% preamble: set title page
\author[SONG]{Song Hongwei}
\title{Paper Notes}
\institute{Harbin Institute of Techonology}
\date{\today}

% inside document env
% add title page
\frame{\titlepage}

% Table of content
\begin{frame}
\frametitle{Table of Contents}
\tableofcontents[currentsection]
\end{frame}

% section, subsection before or between frames
\section

% References
\begin{frame}[t,allowframebreaks]
\frametitle{References}
	\bibliographystyle{plainnat}
	\bibliography{mybib}
\end{frame}
```
* figures
```
\begin{center}
	\includegraphics[height=0.8\textheight]{figures/myfig}
\end{center}
```
* text figure side-by-side
```
\begin{columns}
	\begin{column}{0.4\textwidth}
		# text here
	\end{column}
	\begin{column}{0.6\textwidth}
		# image here
	\end{column}
\end{columns}
```

* block
```
\begin{alertblock}{Comment}
attention is effective for domain mismatch.
\end{alertblock}
```

* beamer enable bibliography numbering
```
# in preamble part, add this line
\setbeamertemplate{bibliography item}{\insertbiblabel}
```
## notes
* Not in math
```
{\rm sigmoid}(x)
```

## code blocks
* `listings` package
```
\usepackage{listings}
\usepackage{xcolor}
 
\definecolor{codegreen}{rgb}{0,0.6,0}
\definecolor{codegray}{rgb}{0.5,0.5,0.5}
\definecolor{codepurple}{rgb}{0.58,0,0.82}
\definecolor{backcolour}{rgb}{0.95,0.95,0.92}
 
\lstdefinestyle{mystyle}{
    backgroundcolor=\color{backcolour},   
    commentstyle=\color{codegreen},
    keywordstyle=\color{magenta},
    numberstyle=\tiny\color{codegray},
    stringstyle=\color{codepurple},
    basicstyle=\ttfamily\footnotesize,
    breakatwhitespace=false,         
    breaklines=true,                 
    captionpos=b,                    
    keepspaces=true,                 
    numbers=left,                    
    numbersep=5pt,                  
    showspaces=false,                
    showstringspaces=false,
    showtabs=false,                  
    tabsize=2
}
 
\lstset{style=mystyle}
```
