include ../Makefile.inc
UPSTREAM=https://ftp.postgresql.org/pub/source/v9.6.11/postgresql-9.6.11.tar.gz
TARBALL=$(notdir $(UPSTREAM))

all: bin/postgresql

bin/postgresql: build/postgresql

build/postgresql: build/Makefile
	$(MAKE) -C build MAKELEVEL=0
	$(MAKE) install -C build

POSTGRESQL_CONF_OPTS += \
	--prefix=/none \
	--disable-shared \
	--without-readline \
	--with-z=$(RUMPRUN_SYSROOT) \
	--with-openssl

build/Makefile: build/configure
	(cd build; \
	CC=$(RUMPRUN_CC) ./configure --host=$(RUMPRUN_TOOLCHAIN_TUPLE) $(POSTGRESQL_CONF_OPTS);)

dl/$(TARBALL):
	mkdir -p dl
	../scripts/fetch.sh ${UPSTREAM} dl/$(TARBALL)

build/configure: | dl/$(TARBALL)
	mkdir -p build
	(cd build && tar -zx --strip-components 1 -f ../dl/$(TARBALL))

.PHONY: clean
clean:
	-$(MAKE) -C build clean

.PHONY: distclean
distclean: clean
	rm -rf build
	rm -rf .deps

