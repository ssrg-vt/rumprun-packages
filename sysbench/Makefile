include ../Makefile.inc
UPSTREAM=https://downloads.mysql.com/source/sysbench-0.4.12.14.tar.gz

all: bin/sysbench

bin/sysbench: build/sysbench/sysbench
	mkdir -p bin
	cp $< $@

build/sysbench/sysbench: build/Makefile
	../scripts/apply-patches.sh build/ sysbench.patch
	$(MAKE) -C build
#../scripts/apply-patches.sh build/ sysbench_timer.patch.new.new

build/Makefile: build/configure
	(cd build; ./configure --host=$(RUMPRUN_TOOLCHAIN_TUPLE) --without-mysql)

build/configure:
	mkdir -p build
	../scripts/fetch.sh ${UPSTREAM} build/sysbench-0.4.12.14.tar.gz
	(cd build && tar -xz --strip-components 1 -f sysbench-0.4.12.14.tar.gz)

.PHONY: clean
clean:
	-$(MAKE) -C build clean
	rm -f bin/*

.PHONY: distclean
distclean:
	-$(MAKE) -C build distclean
