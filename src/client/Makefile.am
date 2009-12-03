SUBDIRS = . tests

INCLUDES = -I$(top_srcdir)                                                    \
           -I$(top_srcdir)/src                                                \
           -I$(top_srcdir)/src/libply                                         \
           -I$(srcdir)

plymouthdir = $(plymouthclientdir)

plymouth_PROGRAMS = plymouth

plymouth_CFLAGS = $(PLYMOUTH_CFLAGS) -DPLYMOUTH_PLUGIN_PATH=\"$(PLYMOUTH_PLUGIN_PATH)\"
plymouth_LDADD = $(PLYMOUTH_LIBS) ../libply/libply.la
plymouth_SOURCES = \
                      $(srcdir)/../ply-boot-protocol.h                        \
                      $(srcdir)/ply-boot-client.h                             \
                      $(srcdir)/ply-boot-client.c                             \
                      $(srcdir)/plymouth.c

lib_LTLIBRARIES = libplybootclient.la

libplybootclient_la_CFLAGS = $(PLYMOUTH_CFLAGS)
libplybootclient_la_LIBADD = $(PLYMOUTH_LIBS) ../libply/libply.la
libplybootclient_la_LDFLAGS = -export-symbols-regex '^[^_].*' \
		-version-info $(LT_CURRENT):$(LT_REVISION):$(LT_AGE) \
		-no-undefined
libplybootclient_la_SOURCES = \
                      $(srcdir)/ply-boot-client.h                             \
                      $(srcdir)/ply-boot-client.c

libplybootclientdir = $(includedir)/plymouth-1/plybootclient
libplybootclient_HEADERS = \
			ply-boot-client.h

if WITH_RHGB_COMPAT_LINK
install-data-hook:
	(mkdir -p $(DESTDIR)$(bindir) && cd $(DESTDIR)$(bindir) && ln -sf ../../bin/plymouth rhgb-client)

uninstall-hook:
	rm -f $(DESTDIR)$(bindir)/rhgb-client
	-rmdir -p $(DESTDIR)$(bindir)/rhgb-client >& /dev/null
endif

MAINTAINERCLEANFILES = Makefile.in
