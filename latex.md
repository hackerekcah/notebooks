# Latex (`la tech`)

## install latex and editors on windows
* install MikTex on windows
* MikTex is a LaTeX distribution, including `tex, latex, pdftex, pdflatex`
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
