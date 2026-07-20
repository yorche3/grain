# Hello, World! — Grain

Implementación de la especificación [01_Hello_World](https://yorche3.github.io/programming_languages/core/foundations/01_Hello_World/) en **Grain**, con un enfoque **manual y minimalista**.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / File | Propósito / Purpose |
|----------------|---------------------|
| [`hello_world.gr`](hello_world.gr) | Código fuente principal: imprime `"Hello, World! from Grain!"` en la consola. |

**Estructura de directorios esperada:**

```text
grain/
└── core/
    └── foundations/
        └── helloworld/
            ├── hello_world.gr    # Código fuente
            └── README.md         # Este archivo
```

**ES:** La ubicación sigue la convención `{lenguaje}/core/foundations/hello_world/` del repositorio principal. Al no requerir archivos de configuración ni gestor de paquetes para programas simples, el proyecto consta exclusivamente del archivo fuente y su documentación.

**EN:** The location follows the `{language}/core/foundations/hello_world/` convention of the main repository. Since no configuration files or package manager are required for simple programs, the project consists exclusively of the source file and its documentation.

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** Este proyecto fue creado **manualmente**, sin usar `grain init` ni scaffolding. Se optó por el enfoque más simple posible:

1. Crear el directorio `core/foundations/helloworld/` dentro del árbol `grain/`.
2. Escribir el archivo fuente `hello_world.gr` con la estructura mínima de un programa Grain (`module HelloWorld` + `print`).
3. Usar únicamente `print` de la biblioteca estándar (Prelude), sin dependencias externas.

**EN:** This project was created **manually**, without using `grain init` or scaffolding. The simplest possible approach was chosen:

1. Create the `core/foundations/helloworld/` directory inside the `grain/` tree.
2. Write the source file `hello_world.gr` with the minimal Grain program structure (`module HelloWorld` + `print`).
3. Use only `print` from the standard library (Prelude), with no external dependencies.

### Comparación con Ada / Comparison with Ada

| Aspecto | Ada | Grain |
|---------|-----|-------|
| Scaffolding | `alire.toml` + `hello_world.gpr` + `.gitignore` | No requiere archivos de proyecto para programas simples |
| Compilación | `gprbuild -P hello_world.gpr` | `grain compile hello_world.gr` |
| Ejecución | `./bin/hello_world` | `grain run hello_world.gr` o `wasmtime hello_world.gr.wasm` |
| Archivos mínimos | 3 archivos (fuente + proyecto + manifiesto) | 1 archivo (solo fuente) |
| Salida generada | Ejecutable nativo en `bin/` | WebAssembly (`.wasm`) |

> **ES:** Grain compila a **WebAssembly** (`.wasm`), que puede ejecutarse con el runtime de Grain o con cualquier runtime WASM compatible (wasmtime, wasmer, etc.).
>
> **EN:** Grain compiles to **WebAssembly** (`.wasm`), which can be run with the Grain runtime or any compatible WASM runtime (wasmtime, wasmer, etc.).

---

## 📄 Archivos de configuración clave / Key Configuration Files

**ES:** Grain no requiere archivos de configuración (como `grain.json`) para programas monofichero que usan solo la biblioteca estándar. El compilador `grain` infiere toda la configuración necesaria del código fuente.

**EN:** Grain does not require configuration files (like `grain.json`) for single-file programs using only the standard library. The `grain` compiler infers all necessary configuration from the source code.

---

## 🚀 Compilación y ejecución / Build & Run

### Ejecutar directamente (compilar y ejecutar en un paso)

```bash
# Desde grain/core/foundations/helloworld/
grain run hello_world.gr
```

**Salida esperada / Expected output:**

```text
Hello, World! from Grain!
```

### Compilar y ejecutar por separado

```bash
# Compilar (genera hello_world.gr.wasm)
grain compile hello_world.gr

# Ejecutar con el runtime de Grain
grain hello_world.gr.wasm

# O ejecutar con wasmtime
wasmtime hello_world.gr.wasm
```

**Salida esperada / Expected output:**

```text
Hello, World! from Grain!
```

### Desde cualquier directorio (ruta relativa)

```bash
# Desde la raíz del repositorio
grain run grain/core/foundations/helloworld/hello_world.gr
```

---

## 📝 Notas de implementación / Implementation Notes

- **ES:** Grain usa `print` de la biblioteca estándar (Prelude) para imprimir en consola, de forma similar a Python. No requiere `import` explícito porque `print` está en el alcance global por defecto.
- **EN:** Grain uses `print` from the standard library (Prelude) to print to the console, similar to Python. No explicit `import` is needed because `print` is in the global scope by default.
- **ES:** Todo archivo Grain debe declarar un módulo con `module NombreDelModulo`. El nombre del módulo no tiene que coincidir con el nombre del archivo, pero por convención se usa el mismo nombre.
- **EN:** Every Grain file must declare a module with `module ModuleName`. The module name does not have to match the file name, but by convention the same name is used.
- **ES:** A diferencia de Go (que compila a ejecutable nativo) o Java/Ada (que también generan código nativo), Grain compila a **WebAssembly** (`.wasm`), lo que permite ejecutar el mismo binario en cualquier plataforma con un runtime WASM.
- **EN:** Unlike Go (which compiles to a native binary) or Java/Ada (which also generate native code), Grain compiles to **WebAssembly** (`.wasm`), allowing the same binary to run on any platform with a WASM runtime.

---

### 🌐 Otras implementaciones / Other implementations

Este proyecto también está implementado en otros lenguajes. Explora el [repositorio principal](https://github.com/yorche3/programming_languages) para ver todas las versiones.

---

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
