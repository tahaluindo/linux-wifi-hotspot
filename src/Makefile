CC=gcc
PKGCONFIG = $(shell which pkg-config)

CFLAGS=`pkg-config --cflags gtk+-3.0`
LIBS=`pkg-config --libs gtk+-3.0 --libs x11` -lstdc++ -lpng -lqrencode

APP_NAME="wihotspot"
APP_GUI_BINARY="wihotspot-gui"

PREFIX=/usr
BINDIR=$(PREFIX)/bin

ODIR=../build

GLIB_COMPILE_RESOURCES = $(shell $(PKGCONFIG) --variable=glib_compile_resources gio-2.0)

BUILT_SRC = resources.c

_OBJ = main.o ui.o h_prop.o util.o read_config.o about_ui.o qr_ui.o qrgen.o $(BUILT_SRC:.c=.o)
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))

# Determine this makefile's path.
# Be sure to place this BEFORE `include` directives, if any.
THIS_FILE := $(lastword $(MAKEFILE_LIST))

.PHONY: clean

all: resources.c

resources.c: ui/glade/wifih.gresource.xml ui/glade/wifih.ui
	$(GLIB_COMPILE_RESOURCES) ui/glade/wifih.gresource.xml --target=ui/$@ --sourcedir=ui/glade --generate-source
	@$(MAKE) -f $(THIS_FILE) wihotspot-gui
	
$(ODIR)/%.o: ui/%.c
	$(CC) -c -o $@ $< $(CFLAGS)

$(ODIR)/%.o: ui/%.cpp
	g++ -c -o $@ $<

wihotspot-gui: $(OBJ)
	$(CC) -o $(ODIR)/$@ $^ $(CFLAGS) $(LIBS)

install: $(ODIR)/wihotspot-gui
	install -Dm644 desktop/icons/hotspot@64.png $(DESTDIR)$(PREFIX)/share/pixmaps/wihotspot.png
	install -Dm644 desktop/icons/hotspot@48.png $(DESTDIR)$(PREFIX)/share/icons/hicolor/48x48/apps/wihotspot.png
	install -Dm644 desktop/icons/hotspot@64.png $(DESTDIR)$(PREFIX)/share/icons/hicolor/64x64/apps/wihotspot.png
	install -Dm644 desktop/icons/hotspot@256.png $(DESTDIR)$(PREFIX)/share/icons/hicolor/256x256/apps/wihotspot.png
	install -Dm644 desktop/icons/hotspot@512.png $(DESTDIR)$(PREFIX)/share/icons/hicolor/512x512/apps/wihotspot.png
	install -Dm644 desktop/icons/hotspot.svg $(DESTDIR)$(PREFIX)/share/icons/hicolor/scalable/apps/wihotspot.svg
	install -Dm644 desktop/wifihotspot.desktop $(DESTDIR)$(PREFIX)/share/applications/$(APP_NAME).desktop
	install -Dm644 scripts/policies/polkit.rules $(DESTDIR)$(PREFIX)/share/polkit-1/rules.d/90-org.opensuse.policykit.wihotspot.rules
	install -Dm644 scripts/policies/polkit.policy $(DESTDIR)$(PREFIX)/share/polkit-1/actions/org.opensuse.policykit.wihotspot.policy
	install -Dm755 $(ODIR)/wihotspot-gui $(DESTDIR)$(BINDIR)/$(APP_GUI_BINARY)
	cd scripts && $(MAKE) install

uninstall:
	rm -f $(DESTDIR)$(PREFIX)/share/pixmaps/wihotspot.png
	rm -f $(DESTDIR)$(PREFIX)/share/icons/hicolor/48x48/apps/wihotspot.png
	rm -f $(DESTDIR)$(PREFIX)/share/icons/hicolor/64x64/apps/wihotspot.png
	rm -f $(DESTDIR)$(PREFIX)/share/icons/hicolor/256x256/apps/wihotspot.png
	rm -f $(DESTDIR)$(PREFIX)/share/icons/hicolor/512x512/apps/wihotspot.png
	rm -f $(DESTDIR)$(PREFIX)/share/icons/hicolor/scalable/apps/wihotspot.svg
	rm -f $(DESTDIR)$(PREFIX)/share/applications/$(APP_NAME).desktop
	rm -f $(DESTDIR)$(PREFIX)/share/polkit-1/rules.d/90-org.opensuse.policykit.wihotspot.rules
	rm -f $(DESTDIR)$(PREFIX)/share/polkit-1/actions/org.opensuse.policykit.wihotspot.policy
	rm -f $(DESTDIR)$(BINDIR)/$(APP_GUI_BINARY)
	cd scripts && $(MAKE) uninstall

clean-old:
	rm -f $(DESTDIR)$(PREFIX)/share/pixmaps/wihotspot.png

clean:
	rm -f $(ODIR)/*.o
	rm -f ui/$(BUILT_SRC)
	rm -f $(ODIR)/wihotspot-gui

