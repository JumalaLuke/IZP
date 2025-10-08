all: main

CC = clang
override CFLAGS += -std=c11 -Wall -Wextra -Wno-unused-variable -Wno-unused-parameter -Wno-unused-result -Wno-unknown-pragmas -pedantic -lm

.PHONY: run test submit clean

main: main.c
	$(CC) $(CFLAGS) -O0 ./*.c -o "$@"

main-debug: main.c
	$(CC) $(CFLAGS) -O0 -g ./*.c -o "$@"

main-test: main.c
	$(CC) $(CFLAGS) -O0 -DTEST_BUILD=1 ./*.c -o "$@"

main-solution: main.c
	$(CC) $(CFLAGS) -O3 -DTEST_BUILD=1 -DSOLUTION_BUILD=1 test.c solution/*.c -o "$@"

run: main
	./main $(ARGS)

test:
	@./.tests/scripts/run_tests.sh

submit:
	@PROJECT_SUBMIT_MODE=sources ECHO_QUIET=1 ./.tests/scripts/run_tests.sh

clean:
	rm -f main main-*
