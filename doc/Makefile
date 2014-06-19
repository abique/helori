all: spec.html overview.svg

spec.html: spec.rst
	rst2html $< >$@

overview.svg: overview.dot
	dot -T svg -o $@ $<

clean:
	rm -f spec.html overview.svg
