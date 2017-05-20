ARCH_HEADER = jit-arch-@JIT_ARCH@.h

BUILT_SOURCES = jit-arch.h

AM_CFLAGS = -I$(top_srcdir)/include -I$(top_builddir)/include \
	-I$(top_srcdir)/jit -I$(top_builddir)/jit

jit-arch.h: $(top_srcdir)/include/jit/$(ARCH_HEADER)
	rm -f $@
	$(LN_S) $(top_srcdir)/include/jit/$(ARCH_HEADER) $@

gen-rules-scanner.c: gen-rules-scanner.l
	$(LEX) -t $< > "$@"

gen-rules-parser.c gen-rules-parser.h: gen-rules-parser.y
	$(YACC) --defines -o gen-rules-parser.c $<

gen-ops-scanner.c: gen-ops-scanner.l 
	$(LEX) -t $< > "$@"

gen-ops-parser.c gen-ops-parser.h: gen-ops-parser.y
	$(YACC) --defines -o gen-ops-parser.c $<

all-local: $(top_builddir)/jit/jit-apply-rules.h

all: \
	gen-apply$(BUILD_EXEEXT) \
	gen-ops$(BUILD_EXEEXT) \
	gen-rules$(BUILD_EXEEXT)

gen-rules$(BUILD_EXEEXT): \
	gen-rules-parser.c \
	gen-rules-scanner.c \
	gen-rules-parser.h
	$(CC_FOR_BUILD) -o $@ $^ -I. -I$(top_builddir) -I$(top_srcdir)/jit

gen-ops$(BUILD_EXEEXT): \
	gen-ops-scanner.c \
	gen-ops-parser.c \
	gen-ops-parser.h
	$(CC_FOR_BUILD) -o $@ $^ -I. -I$(top_builddir) -I$(top_srcdir)/jit

gen-apply$(BUILD_EXEEXT): \
	gen-apply.c \
	gen-apply-helper.c \
	gen-apply-helper.h \
	gen-apply-macosx.h
	$(CC_FOR_BUILD) -o $@ $^ -I. -I$(top_builddir) -I$(top_srcdir)/jit

$(top_builddir)/jit/jit-apply-rules.h: gen-apply$(BUILD_EXEEXT)
	./gen-apply$(BUILD_EXEEXT) >$@

CLEANFILES = $(top_builddir)/jit/jit-apply-rules.h jit-arch.h \
	gen-rules-parser.c gen-rules-parser.h gen-rules-scanner.c \
	gen-ops-parser.c gen-ops-parser.h gen-ops-scanner.c \
	gen-rules$(BUILD_EXEEXT)\
	gen-ops$(BUILD_EXEEXT)\
	gen-apply$(BUILD_EXEEXT)
