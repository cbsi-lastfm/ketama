PREFIX=$(DESTDIR)/usr
CFLAGS+=-std=gnu99 -fPIC -W -Wall -Werror -Wno-stringop-truncation

build:
	gcc $(CFLAGS) -O3 -c md5.c
	gcc $(CFLAGS) -O3 -c ketama.c
	gcc -shared -Wl,-soname,libketama.so.1 -o libketama.so.1 ketama.o md5.o -lm

debug:
	gcc $(CFLAGS) -g -DDEBUG -o md5-debug.o -c md5.c
	gcc $(CFLAGS) -g -DDEBUG -o ketama-debug.o -c ketama.c
	gcc -g -shared -Wl,-soname,libketama.so.1 -o libketama.so.1 ketama-debug.o md5-debug.o -lm

test: build
	gcc $(CFLAGS) -I. -O3 -o ketama_test ketama_test.c ./libketama.so.1 -lm
	@./test.sh
	@./downed_server_test.sh

install:
	install -d $(PREFIX)/lib $(PREFIX)/include
	install libketama.so.1 $(PREFIX)/lib/
	install ketama.h $(PREFIX)/include/
	cd ${PREFIX}/lib && ln -sf libketama.so.1 libketama.so

clean:
	rm -f *.o
	rm -f libketama.so.1
	rm -f ketama_test
	rm -f build-stamp configure-stamp

uninstall:
	rm $(PREFIX)/lib/libketama.so.1
	rm $(PREFIX)/include/ketama.h

deinstall: uninstall
