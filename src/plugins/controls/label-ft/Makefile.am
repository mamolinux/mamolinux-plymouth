AM_CPPFLAGS = -I$(top_srcdir)                                                 \
           -I$(srcdir)/../../../libply                                        \
           -I$(srcdir)/../../../libply-splash-core                            \
           -I$(srcdir)/../../../libply-splash-graphics                        \
           -I$(srcdir)/../../..                                               \
           -I$(srcdir)/../..                                                  \
           -I$(srcdir)/..                                                     \
           -I$(srcdir)

plugindir = $(libdir)/plymouth
plugin_LTLIBRARIES = label-ft.la

label_ft_la_CFLAGS = $(PLYMOUTH_CFLAGS) $(FREETYPE_CFLAGS)

label_ft_la_LDFLAGS = -module -avoid-version -export-dynamic
label_ft_la_LIBADD = $(PLYMOUTH_LIBS) $(FREETYPE_LIBS)                           \
                  ../../../libply/libply.la                                \
                  ../../../libply-splash-core/libply-splash-core.la        \
                  ../../../libply-splash-graphics/libply-splash-graphics.la
label_ft_la_SOURCES = $(srcdir)/plugin.c

MAINTAINERCLEANFILES = Makefile.in
