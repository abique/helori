all: spec.html overview.svg pool-structure.svg

%.html: %.rst
	rst2html $< >$@

%.svg: %.dot
	dot -T svg -o $@ $<

clean:
	rm -f spec.html overview.svg pool-structure.svg
