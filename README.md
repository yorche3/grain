# Grain

Proyectos en **Grain**, un lenguaje funcional que compila a WebAssembly.

---

## 📂 Módulos / Modules

| Módulo | Descripción |
|--------|-------------|
| [`core/foundations/`](core/foundations/) | **Fase 0 — Fundamentos**: `hello_world`, `calculator`, `numbers` |

> **ES:** `hello_user` (02_Hello_User) **no se implementa** en Grain: el lenguaje no ofrece, a la fecha, una forma de leer entrada interactiva desde la consola (`stdin`) en la biblioteca estándar. El resto de los módulos de fundamentos, que no requieren entrada del usuario, sí se implementan con normalidad.
>
> **EN:** `hello_user` (02_Hello_User) **is not implemented** in Grain: the language does not currently provide a way to read interactive console input (`stdin`) in its standard library. The remaining foundations modules, which don't require user input, are implemented normally.

---

## ▶️ Comenzar / Getting Started

```bash
# Hello, World!
cd core/foundations/helloworld
grain run hello_world.gr

# Calculator Tests
cd core/foundations/unit_test/calculator
make test

# Numbers Tests
cd core/foundations/numbers
make test
```

---

## 📦 Requisitos / Requirements

| Herramienta | Instalación |
|-------------|-------------|
| [Grain](https://grain-lang.org/) | `npm install -g @grain-lang/cli` / [Descargar](https://grain-lang.org/docs/getting_grain) |

```bash
# Verificar instalación
grain --version
```

---

## 🏗️ Tipos de proyecto / Project Types

### 1. Programa simple (archivo único)

**ES:** Un único archivo `.gr`, sin dependencias externas, ejecutable directamente con `grain run`. Ideal para `hello_world`. No requiere Makefile ni configuración.

**EN:** A single `.gr` file, no external dependencies, executable directly with `grain run`. Ideal for `hello_world`. No Makefile or configuration required.

```bash
grain run archivo.gr
```

### 2. Proyecto con pruebas unitarias (`src/` + `tests/` + Makefile)

**ES:** Para proyectos que requieren pruebas unitarias, se organiza el código fuente en `src/` y las pruebas en `tests/`. Como Grain no tiene un framework de pruebas estándar, se incluye un mini framework propio (`test.gr` o `testing.gr`). Un Makefile unifica compilación, ejecución y limpieza.

**EN:** For projects that require unit tests, source code goes in `src/` and tests in `tests/`. Since Grain has no standard test framework, a custom mini framework is included (`test.gr` or `testing.gr`). A Makefile unifies compilation, execution, and cleanup.

```bash
make build    # compilar
make run      # compilar + ejecutar
make clean    # limpiar artefactos
```

---

## 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
