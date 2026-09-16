# Algorithms Pure — Grain

Implementaciones de la [Fase 1 — Algoritmos Puros](https://yorche3.github.io/programming_languages/ROADMAP/#fase-1--algoritmos-puros--algorithms-pure-) en **Grain**, un lenguaje funcional que compila a **WebAssembly**: ordenamientos elementales, estructuras de datos propias, ordenamientos óptimos y distribuidos, y búsqueda.

Los módulos de esta fase trabajan sobre listas **inmutables** (`List<Number>`): ninguna función ordena *in-place*, todas devuelven una lista nueva.

---

## 📂 Módulos / Modules

| Módulo | Especificación | Enfoque | Tests | Estado |
|--------|---------------|---------|:-----:|:------:|
| [`naive_sort/`](naive_sort/) | [05_Naive_Sort](https://yorche3.github.io/programming_languages/core/algorithms/05_Naive_Sort/) | `make test` + `testing.gr` propio | 21 | ✅ |

---

## 📁 Estructura / Structure

```text
algorithms/
└── naive_sort/                  # 05_Naive_Sort
    ├── Makefile
    ├── .gitignore
    ├── src/
    │   └── naive_sort.gr        # selectionSort, bubbleSort, insertionSort
    ├── tests/
    │   ├── testing.gr           # Mini framework (reutilizado de numbers/)
    │   ├── naive_sort_test.gr   # 3 algoritmos × 7 casos = 21 aserciones
    │   └── run_tests.gr         # Entry point
    └── README.md
```

---

## 🛠️ Patrón común / Common Pattern

| Característica | Descripción |
|---------------|-------------|
| **Runtime** | WebAssembly (`grain compile` → `.wasm`) |
| **CLI** | `grain compile`, `grain run` |
| **Sin manifiesto** | Grain no tiene gestor de paquetes ni archivo de configuración |
| **Build** | `Makefile` con `build`, `run`, `test` y `clean` |
| **Framework de tests** | `testing.gr` propio — Grain no trae framework estándar |
| **Entry point** | `tests/run_tests.gr`, que importa las suites y llama a `printSummary()` |
| **Separación** | `src/` (código) ↔ `tests/` (suites + framework + runner) |
| **Iteración** | Recursión y *pattern matching* sobre `[x, ...xs]`; listas inmutables, sin funciones de biblioteca |
| **Visibilidad** | `provide` para exportar; los helpers se anidan con `let rec` dentro de la función padre |
| **Naming** | `camelCase` (`selectionSort`), módulos en `PascalCase` |
| **Indicador de fallo** | No aplica: `List<a>` no admite entradas inválidas |
| **Artefactos** | `target/`, `*.gro`, `*.wasm`, `*.wasm.map` — ignorados en `.gitignore` |

---

## 🚀 Compilación rápida / Quick Build

```bash
# Naive Sort Tests
cd naive_sort
make test
```

---

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

## ▶️ Siguiente / Next

👉 Continúa con los módulos pendientes de esta fase en el [Roadmap](https://yorche3.github.io/programming_languages/ROADMAP/).
👉 Continue with the pending modules of this phase in the [Roadmap](https://yorche3.github.io/programming_languages/ROADMAP/).

---

*[← Volver a Core](../README.md)*

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
