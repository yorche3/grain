# Numbers — Grain

Implementación de la especificación [04_Numbers](https://yorche3.github.io/programming_languages/core/foundations/04_Numbers/) en **Grain**, con los 5 algoritmos numéricos en 3 enfoques progresivos (recursivo directo, recursivo con acumulador e iterativo) y un framework de pruebas propio con resumen global unificado.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directorio | Propósito |
|----------------------|-----------|
| `src/numbers.gr` | Implementación de los 5 algoritmos × 3 enfoques = 15 funciones públicas + helpers privados. |
| `tests/testing.gr` | Framework de pruebas genérico con acumuladores globales y resumen unificado. |
| `tests/numbers_rec_test.gr` | Suite de pruebas del enfoque recursivo (11 casos en 5 tests). |
| `tests/numbers_acc_test.gr` | Suite de pruebas del enfoque con acumulador (11 casos en 5 tests). |
| `tests/numbers_ite_test.gr` | Suite de pruebas del enfoque iterativo (11 casos en 5 tests). |
| `tests/run_tests.gr` | Punto de entrada: importa las 3 suites y muestra el resumen global. |
| `Makefile` | Comandos `build`, `run`, `clean` y `test`. |

Con relación a la implementación base en **Ada**, que separa especificación (`numbers.ads`) e implementación (`numbers.adb`) y requiere un subproyecto `tests/` completo con 3 suites (una por enfoque), cada una con su propia especificación y cuerpo, además de un punto de entrada `tests.adb` que las ejecuta todas mediante `AUnit.Run.Test_Runner`, Grain utiliza un **único archivo** de código fuente con las 15 funciones públicas y un **Makefile** como sistema de construcción. El framework `testing.gr` es propio ya que Grain no tiene un framework de pruebas estándar en su ecosistema.

**Estructura de directorios esperada:**

```text
numbers/
├── src/
│   └── numbers.gr              # 15 funciones públicas (5 ops × 3 enfoques)
├── tests/
│   ├── testing.gr              # Framework de pruebas genérico
│   ├── numbers_rec_test.gr     # Tests: enfoque recursivo (5)
│   ├── numbers_acc_test.gr     # Tests: enfoque con acumulador (5)
│   ├── numbers_ite_test.gr     # Tests: enfoque iterativo (5)
│   └── run_tests.gr            # Punto de entrada
├── Makefile
└── README.md
```

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** El proyecto se creó manualmente, sin herramientas de scaffolding. A diferencia de Ada (que usa `alr init --lib` + `alr init --bin tests`), en Grain no hay un comando `grain init` estándar; los archivos se escriben directamente.

Cada algoritmo se implementa de tres formas distintas:

1. **Recursivo Directo (`...Rec`)**: Basado directamente en la definición matemática, con la palabra clave `rec` para funciones recursivas. Múltiples llamadas recursivas, alto uso de pila.
2. **Recursivo con Acumulador (`...Acc`)**: Expone una función limpia que delega en un helper interno con una sola llamada recursiva por paso, usando `let rec` anidado.
3. **Iterativo (`...Ite`)**: Utiliza bucles `for` o `while`, sin recursión, memoria constante O(1).

**EN:** The project was created manually, without scaffolding tools. Unlike Ada (which uses `alr init --lib` + `alr init --bin tests`), Grain has no standard `grain init` command; files are written directly.

Each algorithm is implemented in three different ways:

1. **Direct Recursive (`...Rec`)**: Based directly on the mathematical definition, using the `rec` keyword for recursive functions. Multiple recursive calls, high stack usage.
2. **Accumulator Recursive (`...Acc`)**: Exposes a clean function that delegates to an internal helper with a single recursive call per step, using nested `let rec`.
3. **Iterative (`...Ite`)**: Uses `for` or `while` loops, no recursion, O(1) constant memory.

---

## 📄 Archivos de configuración clave / Key Configuration Files

**ES:** Grain no requiere archivos de configuración como `alire.toml` (Ada), `deps.edn` (Clojure), `mix.exs` (Elixir), o `go.mod` (Go). El compilador infiere la configuración del código fuente y las rutas de importación relativas. No hay gestor de paquetes externo.

**EN:** Grain does not require configuration files like `alire.toml` (Ada), `deps.edn` (Clojure), `mix.exs` (Elixir), or `go.mod` (Go). The compiler infers configuration from the source code and relative import paths. There is no external package manager.

### `Makefile` – Comandos de compilación, ejecución y limpieza

```makefile
.PHONY: all build run clean

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
```

### `tests/testing.gr` – Framework de pruebas

**ES:** Grain no tiene un framework de pruebas estándar (como AUnit en Ada), por lo que se creó uno propio usando `List` de la biblioteca estándar. Utiliza variables mutables globales para acumular resultados a través de las 3 suites y mostrar un único resumen al final.

**EN:** Grain does not have a standard test framework (like AUnit in Ada), so a custom one was created using `List` from the standard library. It uses global mutable variables to accumulate results across all 3 suites and show a single summary at the end.

```grain
module Testing

from "list" include List

provide record TestCase<a> {
  result: a,
  expected: a,
}

provide record Test<a> {
  name: String,
  cases: List<TestCase<a>>
}

let mut globalPassed = 0
let mut globalFailed = 0
let mut globalTotal = 0

provide let runTests = (testSuite) => {
  List.forEach((test) => {
    List.forEach((case :TestCase<a>) => {
      globalTotal += 1
      let passed = match ((case.result, case.expected)) {
        (r, e) => r == e,
      }
      if (passed) {
        globalPassed += 1
      } else {
        globalFailed += 1
        print("FAILED: " ++ test.name ++ " — expected " ++ toString(case.expected) ++ ", got " ++ toString(case.result))
      }
    }, test.cases)
  }, testSuite)
}

provide let printSummary = () => {
  print("\n--- Test Summary ---")
  print("Tests: " ++ toString(globalPassed) ++ " passed, " ++ toString(globalFailed) ++ " failed, " ++ toString(globalTotal) ++ " total")
  globalFailed == 0
}
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

### Ejecutar tests (alias de `run`)

```bash
make test
```

### Limpiar artefactos generados

```bash
make clean
```

### Comandos directos (sin Makefile)

```bash
# Compilar
grain compile tests/run_tests.gr

# Ejecutar
grain run tests/run_tests.wasm
```

**Salida esperada (todo correcto):**

```
--- Test Summary ---
Tests: 33 passed, 0 failed, 33 total
```

**Salida con errores (solo muestra fallos):**

```
FAILED: fibonacciAcc — expected 0, got 1

--- Test Summary ---
Tests: 32 passed, 1 failed, 33 total
```

> **ES:** Cuando todos los tests pasan, no se imprime nada intermedio — solo el resumen final. Solo se muestran líneas `FAILED:` cuando hay errores.
>
> **EN:** When all tests pass, no intermediate output is printed — only the final summary. `FAILED:` lines are only shown when there are errors.

---

## 🧠 Algoritmos / operaciones (según el módulo)

### 3 enfoques × 5 algoritmos = 15 funciones / 15 tests (33 aserciones)

| Algoritmo | Casos de prueba | `_Rec` | `_Acc` | `_Ite` |
|-----------|----------------|:------:|:------:|:------:|
| `SumFirstN` | `(0) = 0`, `(3) = 6` | ✅ | ✅ | ✅ |
| `Factorial` | `(0) = 1`, `(4) = 24` | ✅ | ✅ | ✅ |
| `Fibonacci` | `(0) = 0`, `(1) = 1`, `(6) = 8` | ✅ | ✅ | ✅ |
| `GreatestCommonDivisor` | `(12, 8) = 4`, `(7, 5) = 1` | ✅ | ✅ | ✅ |
| `LeastCommonMultiple` | `(4, 6) = 12`, `(6, 8) = 24` | ✅ | ✅ | ✅¹ |

> ¹ La función iterativa se llama `leastCommonDivisorIte` (por consistencia con el código existente), aunque semánticamente es el MCM/LCM.

---

## 📝 Notas de implementación / Implementation Notes

### 🔁 Sobre recursión con acumulador y Tail Call Optimization (TCO) / On recursion with accumulator and Tail Call Optimization (TCO)

**ES:**

*Tail recursion* ocurre cuando la llamada recursiva es la última acción que ejecuta una función; después de la llamada no hay más instrucciones, la función devuelve el resultado de la llamada recursiva. La recursión con acumulador consigue esto pasando el estado previo como parámetro a cada llamada, sin dejar trabajo pendiente en la pila.

**Grain no garantiza TCO.** Aunque Grain compila a WebAssembly, el compilador no aplica Tail Call Optimization de forma explícita. La especificación del lenguaje no exige TCO, y la implementación actual del compilador (grainc) no optimiza llamadas terminales. Por lo tanto, las funciones con acumulador (`_Acc`) consumen la misma pila que las recursivas directas.

La implementación con acumulador se conserva únicamente con fines educativos: sirve como puente conceptual entre la recursión directa (más cercana a la definición matemática) y la versión iterativa (más eficiente). Sin embargo, a diferencia de otros lenguajes que sí garantizan TCO (como Elixir en la BEAM), en Grain los acumuladores **tienen tests directos** porque la sintaxis funcional del lenguaje hace que este patrón sea idiomático y vale la pena verificarlo explícitamente.

La implementación de `fibonacciAcc` ilustra la tail recursion genuina:

```grain
provide let fibonacciAcc = (n) => {
    let rec fibonacciHelp = (remaining, a, b) => {
        if (remaining == 0) {
            return a
        } else {
            return fibonacciHelp(remaining - 1, b, a + b)
        }
    }
    fibonacciHelp(n, 0, 1)
}
```

La llamada a `fibonacciHelp(remaining - 1, b, a + b)` es tail recursion: después de la llamada no queda trabajo pendiente. Si Grain implementara TCO en el futuro, estas funciones se beneficiarían automáticamente.

**EN:**

*Tail recursion* occurs when the recursive call is the last action that runs a function; after the call there are no more instructions, the function returns the result of the recursive call. Recursion with accumulator achieves this by passing the previous state as a parameter to each call, without leaving any pending work on the stack.

**Grain does not guarantee TCO.** Although Grain compiles to WebAssembly, the compiler does not explicitly apply Tail Call Optimization. The language specification does not require TCO, and the current compiler implementation (grainc) does not optimize tail calls. Therefore, accumulator functions (`_Acc`) consume the same stack as direct recursion.

The accumulator implementation is preserved only for educational purposes: it serves as a conceptual bridge between the direct recursive (closer to mathematical definition) and the iterative version (more efficient). However, unlike languages that do guarantee TCO (like Elixir on the BEAM), in Grain the accumulators **have direct tests** because the functional syntax of the language makes this pattern idiomatic and worth verifying explicitly.

The `fibonacciAcc` implementation illustrates genuine tail recursion:

```grain
provide let fibonacciAcc = (n) => {
    let rec fibonacciHelp = (remaining, a, b) => {
        if (remaining == 0) {
            return a
        } else {
            return fibonacciHelp(remaining - 1, b, a + b)
        }
    }
    fibonacciHelp(n, 0, 1)
}
```

The call to `fibonacciHelp(remaining - 1, b, a + b)` is tail recursion: after the call there is no pending work. If Grain implements TCO in the future, these functions would benefit automatically.

### 💡 Privacidad de helpers en Grain

**ES:**
En Grain, la visibilidad se controla mediante `provide`:
- Las funciones incluidas en un bloque `provide { ... }` son **públicas** (exportadas).
- Las funciones **no incluidas** en `provide` son **privadas** al módulo.

Los helpers con acumulador (`sumFirstHelp`, `factorialHelp`, `fibonacciHelp`) se declaran dentro de la función padre usando `let rec ...`, lo que los hace privados al ámbito de esa función. Esto contrasta con Ada, donde los helpers son funciones separadas en el cuerpo del paquete (`.adb`) sin estar en la especificación (`.ads`).

**EN:**
In Grain, visibility is controlled via `provide`:
- Functions included in a `provide { ... }` block are **public** (exported).
- Functions **not included** in `provide` are **private** to the module.

The accumulator helpers (`sumFirstHelp`, `factorialHelp`, `fibonacciHelp`) are declared inside the parent function using `let rec ...`, making them private to that function's scope. This contrasts with Ada, where helpers are separate functions in the package body (`.adb`) without being in the specification (`.ads`).

### 💡 Grain compila a WebAssembly

**ES:** A diferencia de Ada (ejecutable nativo), Clojure (JVM) o Elixir (BEAM/ERTS), Grain compila a **WebAssembly** (`.wasm`), lo que permite ejecutar el mismo binario en cualquier plataforma con un runtime WASM. Los archivos `.gro` y el directorio `target/` son artefactos intermedios de compilación que se eliminan con `make clean`.

**EN:** Unlike Ada (native binary), Clojure (JVM), or Elixir (BEAM/ERTS), Grain compiles to **WebAssembly** (`.wasm`), allowing the same binary to run on any platform with a WASM runtime. The `.gro` files and `target/` directory are intermediate build artifacts that are removed with `make clean`.

---

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
