MAINTAINERCLEANFILES    = Makefile.in

AM_CPPFLAGS                = -I$(top_builddir)/include -I$(top_srcdir)/include

lib_LTLIBRARIES         = libsheepdog.la

libsheepdog_la_DEPENDENCIES =

libsheepdog_la_SOURCES  = shared/sheep.c shared/vdi.c shared/ops.c util.c

libsheepdog_la_LDFLAGS  = -avoid-version -shared -module -export-dynamic \
			  -export-symbols-regex 'sd_'
# Workaround for Objects ‘created with both libtool and without’ problem
libsheepdog_la_CPPFLAGS = $(AM_CPPFLAGS) -DNO_SHEEPDOG_LOGGER

pkginclude_HEADERS      = ../include/sheepdog_proto.h shared/sheepdog.h shared/internal.h \
			  ../include/util.h ../include/list.h


lib_LIBRARIES 		= libsheepdog.a

libsheepdog_a_SOURCES  	= shared/sheep.c shared/vdi.c util.c

libsheepdog_a_CPPFLAGS  = $(AM_CPPFLAGS) -DNO_SHEEPDOG_LOGGER


libsd_a_DEPENDENCIES =

noinst_LIBRARIES	= libisa.a libsd.a

libisa_a_SOURCES	= $(shell find isa-l/ -type f -regex ".*\.\(c\|h\|asm\)") \
			  isa-l/erasure_code/Makefile \
			  isa-l/Makefile \
			  isa-l/Makefile.nmake \
			  isa-l/make.inc

libsd_a_SOURCES		= event.c logger.c net.c util.c rbtree.c strbuf.c \
			  sha1.c option.c work.c sockfd_cache.c fec.c \
			  sd_inode.c common.c

libsd_a_LIBADD		= isa-l/bin/ec_base.o \
			  isa-l/bin/ec_highlevel_func.o \
			  isa-l/bin/ec_multibinary.o \
			  isa-l/bin/gf_2vect_dot_prod_sse.o \
		  	  isa-l/bin/gf_3vect_dot_prod_sse.o \
		  	  isa-l/bin/gf_4vect_dot_prod_sse.o \
			  isa-l/bin/gf_5vect_dot_prod_sse.o \
			  isa-l/bin/gf_6vect_dot_prod_sse.o \
			  isa-l/bin/gf_vect_dot_prod_sse.o \
			  isa-l/bin/gf_vect_mul_avx.o \
			  isa-l/bin/gf_vect_mul_sse.o

if BUILD_SHA1_HW
libsd_a_SOURCES		+= sha1_ssse3.S
endif

if BUILD_TRACE
AM_CPPFLAGS		+= -DENABLE_TRACE
endif

# support for GNU Flymake
check-syntax:
	$(COMPILE) -fsyntax-only $(CHK_SOURCES)

check-style:
	@$(CHECK_STYLE) $(libsd_a_SOURCES)

libisa.a:
	cd isa-l/ && $(MAKE) && cd ..

clean-local:
	rm -f *.o gmon.out *.da *.bb *.bbg
	cd isa-l/ && $(MAKE) clean && cd ..
