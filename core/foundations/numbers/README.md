# Numbers — Grain

Implementación de la especificación [04_Numbers](https://yorche3.github.io/programming_languages/core/foundations/04_Numbers/) en **Grain**, con los 5 algoritmos numéricos en 3 enfoques progresivos (recursivo directo, recursivo con acumulador e iterativo) y un framework de pruebas propio.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directorio | Propósito |
|----------------------|-----------|
| `src/numbers.gr` | 15 funciones públicas (5 algoritmos × 3 enfoques) + helpers privados. |
| `tests/testing.gr` | Framework de pruebas con acumuladores globales y resumen unificado. |
| `tests/numbers_rec_test.gr` | Tests del enfoque recursivo (5 tests, 11 aserciones). |
| `tests/numbers_acc_test.gr` | Tests del enfoque con acumulador (5 tests, 11 aserciones). |
| `tests/numbers_ite_test.gr` | Tests del enfoque iterativo (5 tests, 11 aserciones). |
| `tests/run_tests.gr` | Punto de entrada que importa las 3 suites y muestra el resumen global. |
| `Makefile` | Comandos `build`, `run`, `clean` y `test`. |

**Estructura de directorios:**

```text
numbers/
├── src/
│   └── numbers.gr
├── tests/
│   ├── testing.gr
│   ├── numbers_rec_test.gr
│   ├── numbers_acc_test.gr
│   ├── numbers_ite_test.gr
│   └── run_tests.gr
├── Makefile
└── README.md
```

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** Proyecto creado manualmente. Los 3 enfoques son:

| Enfoque | Sufijo | Características |
|---------|--------|-----------------|
| Recursivo directo | `Rec` | Múltiples llamadas recursivas, alto uso de pila. Usa `provide let rec`. |
| Con acumulador | `Acc` | Una llamada recursiva por paso con helper interno (`let rec` anidado). |
| Iterativo | `Ite` | Bucles `for`/`while`, memoria constante O(1). |

**EN:** Project created manually. The 3 approaches are:

| Approach | Suffix | Characteristics |
|----------|--------|-----------------|
| Direct recursive | `Rec` | Multiple recursive calls, high stack usage. Uses `provide let rec`. |
| With accumulator | `Acc` | Single recursive call per step with internal helper (nested `let rec`). |
| Iterative | `Ite` | `for`/`while` loops, constant O(1) memory. |

---

## 📄 Archivos de configuración clave / Key Configuration Files

**ES:** Grain no requiere archivos de configuración. No hay gestor de paquetes externo.

**EN:** Grain does not require configuration files. There is no external package manager.

### `Makefile`

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

---

## 🚀 Compilación y ejecución / Build & Run

```bash
make run      # compila + ejecuta
make build    # solo compilar
make clean    # limpiar artefactos
make test     # alias de run
```

**Salida esperada (33 tests, 0 fallos):**

```
--- Test Summary ---
Tests: 33 passed, 0 failed, 33 total
```

---

## 🧠 Algoritmos / Algorithms

| Algoritmo | Casos | `Rec` | `Acc` | `Ite` |
|-----------|-------|:-----:|:-----:|:-----:|
| `SumFirstN` | `(0)=0`, `(3)=6` | ✅ | ✅ | ✅ |
| `Factorial` | `(0)=1`, `(4)=24` | ✅ | ✅ | ✅ |
| `Fibonacci` | `(0)=0`, `(1)=1`, `(6)=8` | ✅ | ✅ | ✅ |
| `GreatestCommonDivisor` | `(12,8)=4`, `(7,5)=1` | ✅ | ✅ | ✅ |
| `LeastCommonMultiple` | `(4,6)=12`, `(6,8)=24` | ✅ | ✅ | ✅ |

---

## 📝 Notas de implementación / Implementation Notes

### TCO

**ES:** Grain **no garantiza TCO**. Las funciones con acumulador (`_Acc`) consumen la misma pila que las recursivas directas. Se conservan con fines educativos como puente conceptual hacia la versión iterativa.

**EN:** Grain **does not guarantee TCO**. Accumulator functions (`_Acc`) consume the same stack as direct recursion. They are preserved for educational purposes as a conceptual bridge to the iterative version.

### Privacidad

**ES:** Visibilidad controlada mediante `provide`. Los helpers se declaran con `let rec` dentro de la función padre, siendo privados a ese ámbito.

**EN:** Visibility controlled via `provide`. Helpers are declared with nested `let rec`, private to that scope.

### WebAssembly

**ES:** Grain compila a `.wasm`. Los archivos `.gro` y `target/` son artefactos intermedios eliminables con `make clean`.

**EN:** Grain compiles to `.wasm`. The `.gro` files and `target/` directory are intermediate artifacts removable with `make clean`.

---

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
