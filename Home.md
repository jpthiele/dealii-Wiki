Welcome to the dealii wiki! This wiki collects explanations and advice that do not quite fit in the manual (though the two documents link to each other in several places).

A word of caution for contributors: due to limitations with Github's wiki implementation, the table of contents for each individual page is manually generated. In particular, the shell script at

https://github.com/ekalinin/github-markdown-toc

(with a little postprocessing) generated the FAQ table of contents. If you add new entries, then please also update the table of contents accordingly.

# Step by step on how to update the TOC:

```
git clone https://github.com/tjhei/github-markdown-toc
cd github-markdown-toc
./gh-md-toc https://github.com/dealii/dealii/wiki/Frequently-Asked-Question
```

and replace the TOC with the output.
