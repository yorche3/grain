.PHONY: all build run test clean

all: run

build:
	grain compile tests/run_tests.gr

run: build
	grain run tests/run_tests.wasm

test: run

clean:
	rm -rf target/
	rm -f tests/*.wasm tests/*.wasm.map tests/*.gro
	rm -f src/*.gro
