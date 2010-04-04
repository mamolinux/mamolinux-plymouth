themedir = $(datadir)/plymouth/themes/ubuntu-logo
nodist_theme_DATA = ubuntu-logo.plymouth
dist_theme_DATA =   ubuntu-logo.script                                \
                    ubuntu_logo.png                                   \
                    password_field.png                                \
                    progress_dot_off.png                              \
                    progress_dot_on.png



MAINTAINERCLEANFILES = Makefile.in ubuntu-logo.plymouth
CLEANFILES = ubuntu-logo.plymouth

ubuntu-logo.plymouth: $(srcdir)/ubuntu-logo.plymouth.in
	sed -e 's,[@]PLYMOUTH_THEME_PATH[@],$(PLYMOUTH_THEME_PATH),g' \
	       $(srcdir)/ubuntu-logo.plymouth.in > ubuntu-logo.plymouth

EXTRA_DIST = ubuntu-logo.plymouth.in
