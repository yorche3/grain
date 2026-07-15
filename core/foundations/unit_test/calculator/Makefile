.PHONY: all build run clean

all: build

build:
	grain compile tests/calculator_test.gr

run: build
	grain run tests/calculator_test.wasm

clean:
	rm -rf target/
	rm -f tests/*.wasm tests/*.gro tests/*.wasm.map
	rm -f src/*.gro