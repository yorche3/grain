# Naive Sort — Grain

Implementación de la especificación [05_Naive_Sort](https://yorche3.github.io/programming_languages/core/algorithms/05_Naive_Sort/) en **Grain**, con los tres algoritmos elementales de ordenamiento ($O(n^2)$) — **Selection Sort**, **Bubble Sort** e **Insertion Sort** — sobre listas inmutables y un framework de pruebas propio.

Los tres algoritmos devuelven una **lista nueva**: `List<Number>` es inmutable en Grain, así que el `swap` *in-place* del pseudocódigo no es representable y el ordenamiento se construye por recursión con *pattern matching*.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directorio | Propósito |
|----------------------|-----------|
| `src/naive_sort.gr` | Las 3 funciones del contrato, cada una con sus helpers anidados. |
| `tests/testing.gr` | Mini framework de pruebas con acumuladores globales y resumen unificado. |
| `tests/naive_sort_test.gr` | Suite única: 3 algoritmos × 7 casos = 21 aserciones. |
| `tests/run_tests.gr` | Punto de entrada que importa la suite y muestra el resumen global. |
| `Makefile` | Comandos `build`, `run`, `test` y `clean`. |
| `.gitignore` | Artefactos generados (`target/`, `*.gro`, `*.wasm`, `*.wasm.map`). |

**Estructura de directorios:**

```text
naive_sort/
├── src/
│   └── naive_sort.gr
├── tests/
│   ├── testing.gr
│   ├── naive_sort_test.gr
│   └── run_tests.gr
├── Makefile
├── .gitignore
└── README.md
```

A diferencia de `foundations/numbers/`, que reparte los 3 enfoques en 3 archivos de suite importados por `run_tests.gr`, aquí los 3 algoritmos comparten un único contrato `List<Number> -> List<Number>` y una única tabla de casos, así que una sola suite los recorre con un helper común.

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** Proyecto creado manualmente con `mkdir -p src tests` más un `Makefile`, como en `numbers/`. Grain no tiene framework de pruebas estándar, así que se reutiliza el mini framework `testing.gr` de forma literal (mismo contenido, mismos acumuladores globales).

**EN:** Project created manually with `mkdir -p src tests` plus a `Makefile`, as in `numbers/`. Grain has no standard test framework, so the `testing.gr` mini framework is reused verbatim (same content, same global accumulators).

---

## 📄 Configuración clave / Key Configuration

**ES:** Grain no requiere archivos de configuración: no hay manifiesto ni gestor de paquetes. El único archivo de construcción es el `Makefile`.

**EN:** Grain requires no configuration files: there is no manifest or package manager. The only build file is the `Makefile`.

### `Makefile`

```makefile
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
```

### `.gitignore`

**ES:** Se añaden `*.wasm` y `*.wasm.map` además de `target/` y `*.gro`, porque `grain compile` deja el binario `tests/run_tests.wasm` junto a las fuentes. Verificado con `git check-ignore -v` sobre los 5 artefactos.

**EN:** `*.wasm` and `*.wasm.map` are added on top of `target/` and `*.gro`, because `grain compile` leaves the `tests/run_tests.wasm` binary next to the sources. Verified with `git check-ignore -v` on all 5 artifacts.

```gitignore
target/

*.gro
*.wasm
*.wasm.map
```

---

## 🚀 Compilación y ejecución / Build & Run

```bash
make build    # solo compilar
make run      # compilar + ejecutar
make test     # alias de run
make clean    # limpiar artefactos
```

**Salida real / Actual output:**

```text
$ make test
grain compile tests/run_tests.gr
grain run tests/run_tests.wasm

--- Test Summary ---
Tests: 21 passed, 0 failed, 21 total
```

> **ES:** `make build` termina sin salida y con exit 0, sin warnings. La suite ejecuta **21 aserciones**: 3 algoritmos × 7 casos de la especificación.
>
> **EN:** `make build` finishes with no output and exit 0, no warnings. The suite runs **21 assertions**: 3 algorithms × 7 specification cases.

---

## 🧠 Algoritmos y operaciones / Algorithms & Operations

| Algoritmo | Estrategia | Complejidad | In-place |
|-----------|------------|-------------|:--------:|
| `selectionSort` | Extrae el mínimo del tramo no ordenado y lo antepone al resultado | $O(n^2)$ siempre | ❌ |
| `bubbleSort` | Compara e intercambia adyacentes por pasadas, con **salida temprana** mediante la bandera `swapped` | $O(n^2)$ peor/promedio, $O(n)$ mejor | ❌ |
| `insertionSort` | Inserta cada elemento en la sub-lista ya ordenada | $O(n^2)$ peor/promedio, $O(n)$ mejor | ❌ |

### Casos cubiertos / Covered cases

| Caso | Entrada | Salida esperada |
|------|---------|-----------------|
| Lista estándar desordenada | `[5, 2, 9, 1, 5, 6]` | `[1, 2, 5, 5, 6, 9]` |
| Lista ya ordenada | `[1, 2, 3, 4, 5]` | `[1, 2, 3, 4, 5]` |
| Lista en orden inverso | `[5, 4, 3, 2, 1]` | `[1, 2, 3, 4, 5]` |
| Elementos idénticos | `[7, 7, 7, 7]` | `[7, 7, 7, 7]` |
| Con números negativos | `[3, -1, 4, -5, 0]` | `[-5, -1, 0, 3, 4]` |
| Un solo elemento | `[42]` | `[42]` |
| Lista vacía | `[]` | `[]` |

---

## 📝 Notas de implementación / Implementation Notes

### 🧬 Inmutabilidad en lugar de `swap` *in-place* / Immutability instead of in-place swap

**ES:** El pseudocódigo ordena el propio array con `swap(arr, i, j)`. En Grain el literal `[...]` construye una `List<Number>`, que es **inmutable**; el tipo mutable sería `Array`, pero ni el literal idiomático ni el módulo hermano `numbers/` lo usan. Los tres algoritmos construyen listas nuevas con `[x, ...xs]` y *pattern matching*.

**EN:** The pseudocode sorts the array itself with `swap(arr, i, j)`. In Grain the `[...]` literal builds a `List<Number>`, which is **immutable**; the mutable type would be `Array`, but neither the idiomatic literal nor the sibling `numbers/` module uses it. All three algorithms build new lists with `[x, ...xs]` and pattern matching.

### 🆗 Indicador de fallo no representable / Failure indicator not representable

**ES:** La especificación pide devolver el indicador de fallo del lenguaje ante una entrada nula o inválida. Grain no tiene `null`/`nil` y `List<a>` no admite valores inválidos, así que el caso nulo **no es representable** y se omite, igual que en **F#** y **Gleam**. Se conservan los 7 casos de la especificación.

**EN:** The specification requires returning the language's failure indicator for a null or invalid input. Grain has no `null`/`nil` and `List<a>` does not admit invalid values, so the null case **is not representable** and is omitted, as in **F#** and **Gleam**. The 7 specification cases are kept.

### 🔁 La bandera `swapped` viaja en un par / The `swapped` flag travels in a pair

**ES:** El criterio de aceptación exige conservar la optimización de salida temprana. Como no hay variable mutable, `bubblePass` devuelve una tupla `(List<Number>, Bool)`: la lista con una pasada aplicada y si esa pasada hizo algún intercambio. `bubbleSort` repite la pasada sólo mientras la bandera sea `true`, de modo que una lista ya ordenada se resuelve en **una sola pasada** y la bandera nunca se pierde. Es la misma solución que en **Erlang**, **F#** y **Gleam**.

**EN:** The acceptance criteria require preserving the early-exit optimization. Since there is no mutable variable, `bubblePass` returns a `(List<Number>, Bool)` tuple: the list after one pass and whether that pass performed any swap. `bubbleSort` repeats the pass only while the flag is `true`, so an already sorted list is resolved in **a single pass** and the flag is never lost. This is the same solution as in **Erlang**, **F#** and **Gleam**.

### 🏷️ Naming, visibilidad y cero dependencias / Naming, visibility and zero dependencies

**ES:** La especificación nombra las funciones en `snake_case` (`selection_sort`); Grain usa `camelCase` en la biblioteca estándar y en `numbers/`, así que la API es `selectionSort`, `bubbleSort` e `insertionSort`, las tres exportadas con `provide`. Los helpers (`pickMin`, `removeFirst`, `bubblePass`, `insert`, `loop`) se declaran con `let rec` **dentro de la función padre**, tal como hace `numbers/`, de modo que son privados a ese ámbito y la única API pública son las 3 funciones del contrato.

El módulo **no importa ningún módulo de la biblioteca estándar**: no hay `from "list" include List` ni ninguna llamada a `List.*`. Todo se resuelve con literales `[...]`, *pattern matching* `[x, ...xs]`, comparaciones y recursión, en línea con el criterio de aceptación de la especificación (nada de bibliotecas de ordenamiento). Como la recursión vive en un helper anidado, `insertionSort` no necesita la palabra clave `rec`, igual que el enfoque con acumulador de `numbers/`.

**EN:** The specification names the functions in `snake_case` (`selection_sort`); Grain uses `camelCase` in its standard library and in `numbers/`, so the API is `selectionSort`, `bubbleSort` and `insertionSort`, all three exported with `provide`. The helpers (`pickMin`, `removeFirst`, `bubblePass`, `insert`, `loop`) are declared with `let rec` **inside the parent function**, exactly as `numbers/` does, so they are private to that scope and the only public API is the 3 contract functions.

The module **imports no standard-library module**: there is no `from "list" include List` and no `List.*` call whatsoever. Everything is resolved with `[...]` literals, `[x, ...xs]` pattern matching, comparisons and recursion, in line with the specification's acceptance criteria (no sorting libraries). Since the recursion lives in a nested helper, `insertionSort` needs no `rec` keyword, just like the accumulator approach in `numbers/`.

### 🧪 Estructura de los tests / Test structure

**ES:** La suite reutiliza el framework `testing.gr` de `numbers/` y sigue el mismo patrón que los demás lenguajes:

- **Constantes con nombre** para cada entrada y salida esperada (`standardInput`, `standardOutput`, `reverseInput`, …), sin duplicar literales.
- Una **tabla de casos** (`SortCase`) con descripción, entrada y salida esperada.
- Un **helper compartido** `assertSortsAllCases(sort, algorithm)` que recibe la función a probar y el nombre del algoritmo y ejecuta los 7 casos con un solo bucle.
- **Una llamada por función** del contrato: `assertSortsAllCases(selectionSort, "selection_sort")`, `…(bubbleSort, "bubble_sort")`, `…(insertionSort, "insertion_sort")`.
- El nombre de cada aserción es `"{algorithm} should sort {case}"`, así que un fallo identifica el algoritmo y el caso (`FAILED: selection_sort should sort an unsorted array`).

**EN:** The suite reuses the `testing.gr` framework from `numbers/` and follows the same pattern as the other languages:

- **Named constants** for every input and expected output (`standardInput`, `standardOutput`, `reverseInput`, …), with no duplicated literals.
- A **case table** (`SortCase`) with description, input and expected output.
- A **shared helper** `assertSortsAllCases(sort, algorithm)` that receives the function under test and the algorithm name and runs the 7 cases in a single loop.
- **One call per contract function**: `assertSortsAllCases(selectionSort, "selection_sort")`, `…(bubbleSort, "bubble_sort")`, `…(insertionSort, "insertion_sort")`.
- Each assertion name is `"{algorithm} should sort {case}"`, so a failure identifies the algorithm and the case (`FAILED: selection_sort should sort an unsorted array`).

### 🔀 Estabilidad de `insert` / `insert` stability

**ES:** `insert` usa `x <= y`, así que un elemento igual no desplaza al ya insertado: la ordenación es **estable**.

**EN:** `insert` uses `x <= y`, so an equal element does not displace the one already inserted: the sort is **stable**.

### ➿ Extracción del mínimo / Minimum extraction

**ES:** `pickMin` devuelve el valor mínimo; `removeFirst` elimina la **primera aparición** de ese valor. No hay ambigüedad observable aunque el mínimo aparezca repetido (caso `[7, 7, 7, 7]`), porque se elimina el valor que se acaba de extraer y el resultado sigue siendo el mismo. Es el mismo criterio que en **Erlang** y **Gleam**.

**EN:** `pickMin` returns the minimum value; `removeFirst` removes the **first occurrence** of that value. There is no observable ambiguity even when the minimum repeats (case `[7, 7, 7, 7]`), because the value just extracted is the one removed and the result is unchanged. This is the same criterion as in **Erlang** and **Gleam**.

### 📍 Desviaciones respecto a la ubicación esperada / Deviations from the expected location

| Especificación | Implementación | Motivo |
|----------------|----------------|--------|
| `test/naive_sort_test.ext` | `tests/naive_sort_test.gr` | El módulo hermano `foundations/numbers` usa `tests/` en plural; la convención existente prevalece. |
| `test/run_tests.ext` | `tests/run_tests.gr` | El punto de entrada existe y usa la extensión `.gr`; se le añade `testing.gr` como framework. |

**ES:** WebAssembly: `grain compile` genera `tests/run_tests.wasm`, `*.gro` y `target/`, todos ignorados por `.gitignore` y eliminables con `make clean`. El módulo no deja `main` de ejemplo, ya que es una biblioteca.

**EN:** WebAssembly: `grain compile` produces `tests/run_tests.wasm`, `*.gro` and `target/`, all ignored by `.gitignore` and removable with `make clean`. The module ships no example `main`, since it is a library.

**ES:** Este proyecto también está implementado en otros lenguajes. Explora el repositorio principal para consultar las demás versiones.

**EN:** This project is also implemented in other languages. Explore the main repository to see the other versions.

---

*[← Volver a Algoritmos Puros](../README.md) · [↑ Volver a Core](../../README.md)*

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
