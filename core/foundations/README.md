# 🚀 Foundations — Grain

Implementaciones de la [Fase 0 — Fundamentos](https://yorche3.github.io/programming_languages/ROADMAP/#fase-0--fundamentos--foundations--completada) en **Grain**: `helloworld`, `unit_test/calculator` y `numbers`.

> **ES:** `hellouser` (02_Hello_User) no se implementa: Grain no ofrece, a la fecha, lectura de entrada interactiva (`stdin`) en su biblioteca estándar. Ver [`grain/README.md`](../../../README.md) para más detalle.
> **EN:** `hellouser` (02_Hello_User) is not implemented: Grain does not currently provide interactive input reading (`stdin`) in its standard library. See [`grain/README.md`](../../../README.md) for details.

---

## 📁 Estructura / Structure

```text
foundations/
├── helloworld/                # 01_Hello_World — manual (archivo único)
│   ├── hello_world.gr
│   └── README.md
│
├── unit_test/
│   └── calculator/            # 03_Unit_Test_Calculator — src/ + tests/ + Makefile
│       ├── src/
│       ├── tests/
│       ├── Makefile
│       └── README.md
│
└── numbers/                   # 04_Numbers — src/ + tests/ + Makefile
    ├── src/
    ├── tests/
    ├── Makefile
    └── README.md
```

---

## 📖 Módulos / Modules

| Módulo | Especificación | Enfoque | Estado |
|--------|---------------|---------|--------|
| `helloworld` | [01_Hello_World](https://yorche3.github.io/programming_languages/core/foundations/01_Hello_World/) | Manual (archivo único) | ✅ |
| `hellouser` | [02_Hello_User](https://yorche3.github.io/programming_languages/core/foundations/02_Hello_User/) | — | ⛔ No aplica (sin `stdin` en Grain) |
| `unit_test/calculator` | [03_Unit_Test_Calculator](https://yorche3.github.io/programming_languages/core/foundations/03_Unit_Test_Calculator/) | `src/` + `tests/` + Makefile | ✅ |
| `numbers` | [04_Numbers](https://yorche3.github.io/programming_languages/core/foundations/04_Numbers/) | `src/` + `tests/` + Makefile | ✅ |

---

## ▶️ Siguiente / Next

👉 Después de fundamentos, continúa con [Fase 1 — Algoritmos Puros](https://yorche3.github.io/programming_languages/ROADMAP/#fase-1--algoritmos-puros--algorithms-pure-).
👉 After foundations, continue with [Phase 1 — Algorithms Pure](https://yorche3.github.io/programming_languages/ROADMAP/#fase-1--algoritmos-puros--algorithms-pure-).

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*[← Volver a Grain](../../README.md)*

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
