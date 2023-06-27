all:
	pandoc -s --toc --toc-depth=3 CH7.md CH8.md CH9.md CH10.md CH11.md -o CH1.tex -H header.tex --template template.tex
	xelatex --shell-escape CH1.tex

