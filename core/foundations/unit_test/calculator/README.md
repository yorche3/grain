# Calculator — Grain

Implementación de la especificación [03_Unit_Test_Calculator](https://yorche3.github.io/programming_languages/core/foundations/03_Unit_Test_Calculator/) en **Grain**, con un mini framework de pruebas propio y un Makefile para compilación y limpieza.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directorio | Propósito |
|----------------------|-----------|
| `src/calculator.gr` | Implementación de las 5 operaciones aritméticas básicas en el módulo `Calculator`. |
| `tests/test.gr` | Mini framework de pruebas con `addTest`, `runTests` y aserciones. |
| `tests/calculator_test.gr` | Suite de pruebas unitarias (5 tests) que importa `test.gr` y se auto-ejecuta. |
| `Makefile` | Comandos `build`, `run` y `clean`. |

**Estructura de directorios esperada:**

```text
calculator/
├── src/
│   └── calculator.gr             # Implementación de las 5 operaciones
├── tests/
│   ├── calculator_test.gr        # Pruebas unitarias (5 tests)
│   └── test.gr                   # Mini framework de pruebas
├── Makefile
└── README.md                     # Este archivo
```

**ES:** A diferencia de otros lenguajes que usan un framework de pruebas externo (testify en Go, Criterion en C, Google Test en C++, AUnit en Ada), Grain no cuenta con un framework de pruebas estándar en su ecosistema. Por ello, se creó un mini framework propio (`test.gr`) que proporciona las funciones `addTest` y `runTests`. El archivo `calculator_test.gr` es auto-ejecutable: al ser cargado por el compilador, registra los 5 tests y llama a `runTests()` automáticamente.

**EN:** Unlike other languages that use an external test framework (testify in Go, Criterion in C, Google Test in C++, AUnit in Ada), Grain does not have a standard test framework in its ecosystem. Therefore, a custom mini framework (`test.gr`) was created, providing `addTest` and `runTests` functions. The `calculator_test.gr` file is self-executing: when loaded by the compiler, it registers the 5 tests and calls `runTests()` automatically.

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** El proyecto fue creado **manualmente**, sin usar `grain init`. Se optó por el enfoque más simple posible:

1. Escribir el módulo `Calculator` en `src/calculator.gr` con las 5 operaciones.
2. Crear un mini framework de pruebas en `tests/test.gr` usando `List` de la stdlib.
3. Escribir la suite `tests/calculator_test.gr` que importa ambos y se auto-ejecuta.
4. Crear un `Makefile` para compilar, ejecutar y limpiar.

Grain compila a **WebAssembly** (`.wasm`), que se ejecuta con su runtime integrado (`grain run`).

**EN:** The project was created **manually**, without using `grain init`. The simplest approach was chosen:

1. Write the `Calculator` module in `src/calculator.gr` with the 5 operations.
2. Create a mini test framework in `tests/test.gr` using `List` from stdlib.
3. Write the `tests/calculator_test.gr` suite that imports both and self-executes.
4. Create a `Makefile` to compile, run, and clean.

Grain compiles to **WebAssembly** (`.wasm`), which runs with its built-in runtime (`grain run`).

---

## 📄 Archivos de configuración clave / Key Configuration Files

**ES:** Grain no requiere archivos de configuración como `grain.json` o `go.mod` para este proyecto. El compilador infiere la configuración del código fuente y las rutas de importación relativas.

**EN:** Grain does not require configuration files like `grain.json` or `go.mod` for this project. The compiler infers configuration from the source code and relative import paths.

### `Makefile` – Comandos de compilación, ejecución y limpieza

```makefile
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
```

---

## 🚀 Compilación y ejecución / Build & Run

### Compilar y ejecutar (recomendado)

```bash
make run
```

### Solo compilar

```bash
make build
```

### Limpiar artefactos generados

```bash
make clean
```

### Comandos directos (sin Makefile)

```bash
# Compilar
grain compile tests/calculator_test.gr

# Ejecutar
grain run tests/calculator_test.wasm
```

**Salida esperada / Expected output:**

```
Test addition(2, 3) = 5 PASSED.
Test subtraction(5, 2) = 3 PASSED.
Test multiplication(3, 4) = 12 PASSED.
Test division(10, 3) = 3 PASSED.
Test modulus(10, 3) = 1 PASSED.
```

---

## 🧠 Algoritmos / operaciones (según el módulo)

| Función / Function | Implementación / Implementation | Cumple / Complies |
|--------------------|--------------------------------|-------------------|
| `addition(a, b)` | `a + b` (suma directa / direct addition) | ✅ |
| `subtraction(a, b)` | `a - b` (resta directa / direct subtraction) | ✅ |
| `multiplication(a, b)` | Suma repetitiva llamando a `addition()` / Repeated addition calling `addition()` | ✅ No usa `*` |
| `division(a, b)` | Resta repetitiva con `>=` llamando a `subtraction()` y `addition()` / Repeated subtraction | ✅ No usa `/` |
| `modulus(a, b)` | `subtraction(a, multiplication(b, division(a, b)))` | ✅ No usa `%` |

> **ES:** `division` usa `while (aCopy >= b)` (con `>=` en lugar de `>`) para manejar correctamente divisiones exactas. `modulus` se implementa como `a - (b * quotient)`, donde `quotient = division(a, b)`.

---

## 📝 Notas de implementación / Implementation Notes

- **ES:** Grain no tiene un framework de pruebas estándar. El mini framework en `test.gr` usa `List` (de la biblioteca estándar) para almacenar los casos de prueba y `List.forEach` para ejecutarlos.
- **EN:** Grain does not have a standard test framework. The mini framework in `test.gr` uses `List` (from the standard library) to store test cases and `List.forEach` to run them.
- **ES:** El módulo `calculator_test.gr` usa `use Calculator.*` y `use Test.*` para poder llamar a las funciones sin prefijo (ej: `addition(2, 3)` en lugar de `Calculator.addition(2, 3)`).
- **EN:** The `calculator_test.gr` module uses `use Calculator.*` and `use Test.*` to call functions without prefix (e.g., `addition(2, 3)` instead of `Calculator.addition(2, 3)`).
- **ES:** Grain compila a WebAssembly (`.wasm`). Los archivos `.gro` y el directorio `target/` son artefactos intermedios de compilación que se eliminan con `make clean`.
- **EN:** Grain compiles to WebAssembly (`.wasm`). The `.gro` files and `target/` directory are intermediate build artifacts that are removed with `make clean`.
- **ES:** `calculator_test.gr` es auto-ejecutable: llama a `Test.runTests()` al final del módulo, que se ejecuta al ser cargado por el compilador.
- **EN:** `calculator_test.gr` is self-executing: it calls `Test.runTests()` at the end of the module, which runs when loaded by the compiler.

---

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
