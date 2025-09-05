# 크로스 컴파일러 설정
CC=arm-ostl-linux-gnueabi-gcc
AR=arm-ostl-linux-gnueabi-ar

CFLAGS=-g -Os -Wall -pedantic --sysroot=/opt/st/myir-yf13x/4.0.4-snapshot/sysroots/cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi
CFLAGS += -mfloat-abi=hard -mfpu=neon-vfpv4 -fPIC
LDFLAGS += --sysroot=/opt/st/myir-yf13x/4.0.4-snapshot/sysroots/cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi 
LDFLAGS += -mfloat-abi=hard -mfpu=neon-vfpv4 -shared

VERSION_MAJOR=1
VERSION_MINOR=0
VERSION_PATCH=0
SO_NAME=libOpenV2G.so
FULL_SO_NAME=$(SO_NAME).$(VERSION_MAJOR).$(VERSION_MINOR).$(VERSION_PATCH)
SONAME=$(SO_NAME).$(VERSION_MAJOR)

PREFIX=$(CURDIR)/build
LIBDIR=$(PREFIX)/lib
INCDIR=$(PREFIX)/include

SOURCES=$(wildcard src/appHandshake/*.c src/codec/*.c src/din/*.c src/iso1/*.c src/iso2/*.c src/transport/*.c src/xmldsig/*.c)
HEADERS=$(wildcard src/appHandshake/*.h src/codec/*.h src/din/*.h src/iso2/*.h src/iso2/*.h src/transport/*.h src/xmldsig/*.h)
OBJECTS=$(SOURCES:.c=.o)
INCLUDEDIRS=$(wildcard src/*)
INCLUDES=$(INCLUDEDIRS:%=-I%)

$(FULL_SO_NAME): $(OBJECTS)
	$(CC) $(LDFLAGS) -Wl,-soname,$(SONAME) -o $@ $(OBJECTS)
	ln -sf $(FULL_SO_NAME) $(SONAME)
	ln -sf $(SONAME) $(SO_NAME)

.c.o:
	$(CC) $(CFLAGS) $(INCLUDES) -c -o $@ $<

install: $(FULL_SO_NAME)
	install -d $(DESTDIR)$(LIBDIR)
	install -m 0755 $(FULL_SO_NAME) $(DESTDIR)$(LIBDIR)/$(FULL_SO_NAME)
	ln -sf $(FULL_SO_NAME) $(DESTDIR)$(LIBDIR)/$(SONAME)
	ln -sf $(SONAME) $(DESTDIR)$(LIBDIR)/$(SO_NAME)

	install -d $(DESTDIR)$(INCDIR)/OpenV2G
	install $(HEADERS) $(DESTDIR)$(INCDIR)/OpenV2G

clean:
	rm -f $(OBJECTS) $(FULL_SO_NAME) $(SONAME) $(SO_NAME)

.PHONY: install clean

