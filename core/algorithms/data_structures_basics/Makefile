.PHONY: all build run clean test

all: run

build:
	grain compile tests/run_tests.gr

run: build
	grain run tests/run_tests.wasm

clean:
	rm -rf target/
	rm -f tests/*.wasm tests/*.gro tests/*.wasm.map
	rm -f src/*.gro

test: run
