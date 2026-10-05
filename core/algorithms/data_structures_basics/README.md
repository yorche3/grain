# Data Structures Basics — Grain

Implementación de la especificación [06_Data_Structures_Basics](https://yorche3.github.io/programming_languages/core/algorithms/06_Data_Structures_Basics/) en **Grain**, con un framework de pruebas personalizado (`testing.gr`) debido a la ausencia de un framework de testing robusto en el ecosistema.

Cuatro estructuras de datos construidas desde cero sobre un único tipo `Node` compartido: **Node** (celda enlazada), **LinkedList** (lista enlazada con punteros a cabeza y cola), **Stack** (pila LIFO) y **Queue** (cola FIFO). Cada ADT gestiona independientemente sus punteros y contador; no hay delegación de unas estructuras en otras ni uso de colecciones de la biblioteca estándar.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directorio | Propósito |
|----------------------|-----------|
| `src/data_structures_basics.gr` | Implementación de `Node`, `LinkedList`, `Stack` y `Queue` sobre el mismo tipo de celda enlazada. |
| `tests/data_structures_basics_test.gr` | Suite de pruebas: 15 casos que cubren los pasos de la especificación para cada estructura. |
| `tests/testing.gr` | Framework de pruebas personalizado con `runTests` y `printSummary`. |
| `tests/run_tests.gr` | Punto de entrada que ejecuta la suite y muestra el resumen. |
| `Makefile` | Comandos de compilación y ejecución. |
| `.gitignore` | Archivos generados excluidos (`target/`, `*.gro`, `*.wasm`, `*.wasm.map`). |

Con relación a la implementación base en **Ada**, que separa especificación (`data_structures_basics.ads`) e implementación (`data_structures_basics.adb`), Grain reúne contrato y código en un único archivo. Grain no tiene un framework de testing estándar, así que se implementó `testing.gr` con funciones `runTests` y `printSummary` que acumulan los resultados y muestran un resumen al final.

**Estructura de directorios esperada:**

```text
data_structures_basics/              # Módulo Grain
├── src/
│   └── data_structures_basics.gr    # Node, LinkedList, Stack, Queue
├── tests/
│   ├── data_structures_basics_test.gr # 15 casos × pasos de la especificación
│   ├── testing.gr                     # Framework personalizado
│   └── run_tests.gr                   # Punto de entrada
├── Makefile                           # Comandos de build y test
├── .gitignore                         # Archivos generados excluidos
└── README.md                          # Este archivo
```

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** El proyecto se creó manualmente, sin herramientas de scaffolding. Grain no tiene un gestor de paquetes ni herramientas de inicialización de proyectos, así que basta con crear los archivos fuente y el `Makefile`.

**EN:** The project was created manually, without scaffolding tools. Grain has no package manager or project initialization tools, so it's enough to create the source files and the `Makefile`.

### Inicialización / Initialization

```bash
# 1. Crear la estructura de directorios / Create directory structure
mkdir -p src tests

# 2. Crear el Makefile / Create the Makefile
# (ver contenido en el repositorio)
```

---

## 📄 Configuración clave / Key Configuration

| Archivo / File | Propósito / Purpose |
|----------------|---------------------|
| `Makefile` | Define los objetivos `build`, `run`, `test` y `clean` usando `grain compile` y `grain run`. |
| `.gitignore` | Excluye los archivos generados: `target/`, `*.gro`, `*.wasm`, `*.wasm.map`. |

No se usan archivos de configuración adicionales. El código fuente vive en `src/` y las pruebas en `tests/`, con un framework personalizado `testing.gr` que proporciona las funciones `runTests` y `printSummary`.

---

## 🚀 Compilación y ejecución / Build & Run

```bash
# Compilar y ejecutar las pruebas / Compile and run tests
make test
```

**Salida real / Actual output:**

```text
grain compile tests/run_tests.gr
grain run tests/run_tests.wasm

▸ Node — caso 1: inicializar y observar valor/enlace
  ✓ get_value(a) = 10
  ✓ el enlace de a sigue ausente
  → 2 passed, 0 failed

▸ Node — caso 2: inicializar otro nodo, enlazar y recorrer
  ✓ get_value(get_next(a)) = 20
  ✓ el enlace de b sigue ausente
  → 2 passed, 0 failed

▸ LinkedList — paso 1: estado vacío
  ✓ is_empty = true
  ✓ size = 0
  ✓ get_head = -1
  → 3 passed, 0 failed

▸ LinkedList — paso 2: insertar por ambos extremos
  ✓ size = 4
  ✓ get_head = 5
  → 2 passed, 0 failed

▸ LinkedList — paso 3: eliminar la primera aparición
  ✓ delete(10) = true
  ✓ get_head = 5
  ✓ size = 3
  → 3 passed, 0 failed

▸ LinkedList — paso 4: valor ausente
  ✓ delete(99) = false
  ✓ get_head no cambia = 5
  ✓ size no cambia = 3
  → 3 passed, 0 failed

▸ LinkedList — paso 5: vaciar la lista
  ✓ delete(5) = true
  ✓ delete(20) = true
  ✓ delete(10) = true
  ✓ is_empty = true
  ✓ size = 0
  ✓ get_head = -1
  → 6 passed, 0 failed

▸ Stack — paso 1: estado vacío y extracción fallida
  ✓ is_empty = true
  ✓ size = 0
  ✓ peek = -1
  ✓ pop = -1
  ✓ la pila sigue vacía tras el pop fallido
  → 5 passed, 0 failed

▸ Stack — paso 2: LIFO y peek no mutante
  ✓ peek = 30
  ✓ size = 3
  → 2 passed, 0 failed

▸ Stack — paso 3: extracción y reutilización
  ✓ pop = 30
  ✓ pop = 40
  ✓ pop = 20
  ✓ pop = 10
  ✓ is_empty = true
  ✓ size = 0
  → 6 passed, 0 failed

▸ Stack — paso 4: vacío tras extracción
  ✓ pop = -1
  ✓ is_empty sigue true
  → 2 passed, 0 failed

▸ Queue — paso 1: estado vacío y extracción fallida
  ✓ is_empty = true
  ✓ size = 0
  ✓ peek = -1
  ✓ dequeue = -1
  ✓ la cola sigue vacía tras el dequeue fallido
  → 5 passed, 0 failed

▸ Queue — paso 2: FIFO y peek no mutante
  ✓ peek = 10
  ✓ size = 3
  → 2 passed, 0 failed

▸ Queue — paso 3: extracción y reutilización
  ✓ dequeue = 10
  ✓ dequeue = 20
  ✓ dequeue = 30
  ✓ dequeue = 40
  ✓ is_empty = true
  ✓ size = 0
  → 6 passed, 0 failed

▸ Queue — paso 4: vacío tras extracción
  ✓ dequeue = -1
  ✓ is_empty sigue true
  → 2 passed, 0 failed

--- Test Summary ---
Cases: 51 passed, 0 failed, 51 total
```

---

## 🧠 Algoritmos y operaciones / Algorithms & Operations

### Node

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `nodeInit(value: Number)` | `Number → Node` | `O(1)` | Crea un nodo con valor `value` y enlace `None`. Equivalente a `init(value)`. |
| `nodeValue(node: Node)` | `Node → Number` | `O(1)` | Devuelve el valor del nodo. Equivalente a `get_value()`. |
| `nodeNext(node: Node)` | `Node → Option<Node>` | `O(1)` | Devuelve el nodo enlazado o `None` si no hay enlace. Equivalente a `get_next()`. |
| `nodeWithNext(node: Node, next: Node)` | `Node, Node → Node` | `O(1)` | Devuelve un nodo nuevo enlazado con `next`. Equivalente a `set_next(next)`. |

### LinkedList

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `linkedListInit()` | `→ LinkedList` | `O(1)` | Lista vacía: `head=None`, `tail=None`, `count=0`. Equivalente a `init()`. |
| `linkedListHead(list: LinkedList)` | `LinkedList → Number` | `O(1)` | Devuelve el valor de la cabeza o `-1` si la lista está vacía. Equivalente a `get_head()`. |
| `linkedListInsertHead(list: LinkedList, value: Number)` | `LinkedList, Number → LinkedList` | `O(1)` | Inserta al principio. Equivalente a `insert_head(value)`. |
| `linkedListInsertTail(list: LinkedList, value: Number)` | `LinkedList, Number → LinkedList` | `O(n)` | Inserta al final. Equivalente a `insert_tail(value)`. **Adaptación**: Grain es inmutable, así que se reconstruye el camino hasta la cola. |
| `linkedListDelete(list: LinkedList, value: Number)` | `LinkedList, Number → (Bool, LinkedList)` | `O(n)` | Elimina la primera aparición de `value`. Devuelve `(true, lista)` si lo eliminó, `(false, misma lista)` si no está. Equivalente a `delete(value)`. |
| `linkedListIsEmpty(list: LinkedList)` | `LinkedList → Bool` | `O(1)` | Devuelve `true` si la lista está vacía. Equivalente a `is_empty()`. |
| `linkedListSize(list: LinkedList)` | `LinkedList → Number` | `O(1)` | Devuelve el número de nodos. Equivalente a `size()`. |

### Stack

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `stackInit()` | `→ Stack` | `O(1)` | Pila vacía: `top=None`, `count=0`. Equivalente a `init()`. |
| `stackPush(stack: Stack, value: Number)` | `Stack, Number → Stack` | `O(1)` | Apila un valor. Equivalente a `push(value)`. |
| `stackPop(stack: Stack)` | `Stack → (Number, Stack)` | `O(1)` | Desapila y devuelve `(valor, pila)`; si la pila está vacía, `(-1, misma pila)`. Equivalente a `pop()`. |
| `stackPeek(stack: Stack)` | `Stack → Number` | `O(1)` | Devuelve el tope sin extraerlo, o `-1` si la pila está vacía. Equivalente a `peek()`. |
| `stackIsEmpty(stack: Stack)` | `Stack → Bool` | `O(1)` | Devuelve `true` si la pila está vacía. Equivalente a `is_empty()`. |
| `stackSize(stack: Stack)` | `Stack → Number` | `O(1)` | Devuelve el número de elementos. Equivalente a `size()`. |

### Queue

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `queueInit()` | `→ Queue` | `O(1)` | Cola vacía: `front=None`, `rear=None`, `count=0`. Equivalente a `init()`. |
| `queueEnqueue(queue: Queue, value: Number)` | `Queue, Number → Queue` | `O(n)` | Encola un valor al final. Equivalente a `enqueue(value)`. **Adaptación**: Grain es inmutable, así que se reconstruye el camino hasta el `rear`. |
| `queueDequeue(queue: Queue)` | `Queue → (Number, Queue)` | `O(1)` | Desencola y devuelve `(valor, cola)`; si la cola está vacía, `(-1, misma cola)`. Equivalente a `dequeue()`. |
| `queuePeek(queue: Queue)` | `Queue → Number` | `O(1)` | Devuelve el frente sin extraerlo, o `-1` si la cola está vacía. Equivalente a `peek()`. |
| `queueIsEmpty(queue: Queue)` | `Queue → Bool` | `O(1)` | Devuelve `true` si la cola está vacía. Equivalente a `is_empty()`. |
| `queueSize(queue: Queue)` | `Queue → Number` | `O(1)` | Devuelve el número de elementos. Equivalente a `size()`. |

---

## 🧩 Decisiones de diseño / Design decisions

| Decisión / Decision | Alternativa considerada / Alternative | Razón / Reason |
|---|---|---|
| Un único tipo `Node` compartido por las tres estructuras | Tipos de nodo separados para cada ADT | La especificación exige un único `Node` compartido; cada ADT gestiona sus propios punteros (`head`/`tail`, `top`, `front`/`rear`) y contador. |
| Funciones puras que devuelven valores nuevos | Mutación de la instancia recibida | Grain es inmutable; no hay mutación de estado. Cada operación devuelve una estructura nueva con los cambios. |
| `Option<Node>` para representar la ausencia de enlace | Usar `-1` o un centinela | Grain tiene tipos algebraicos; `Option<Node>` con `Some(node)` y `None` es la representación idiomática de ausencia. |
| Tuplas `(Bool, LinkedList)` y `(Number, Stack/Queue)` para operaciones que extraen | Excepciones o resultados separados | Grain no tiene excepciones; las tuplas permiten devolver el valor y la estructura resultante en una sola operación, conservando la inmutabilidad. |
| Framework de pruebas personalizado `testing.gr` | Usar un framework externo | Grain no tiene un framework de testing estándar; se implementó uno simple con `runTests` y `printSummary` que acumula los resultados. |
| Recursión de cola para `relink`, `lastNode`, `collectToTail`, `takePrefix` | Recursión simple | Grain garantiza la optimización de recursión de cola (TCO); usar recursión de cola evita desbordamientos de pila en listas grandes. |

---

## 🔀 Adaptaciones idiomáticas / Idiomatic adaptations

| Especificación / Specification | Adaptación / Adaptation | Justificación / Justification |
|---|---|---|
| `init()` para inicializar cada estructura | Funciones `linkedListInit()`, `stackInit()`, `queueInit()` que devuelven la estructura vacía | Grain es inmutable; no hay constructores ni mutación. Las funciones `init` devuelven una nueva instancia con los campos en su estado inicial. |
| `Node.init(value)` | `nodeInit(value: Number)` | Grain usa funciones puras en lugar de métodos de instancia. `nodeInit` devuelve un nuevo `Node` con el valor y el enlace ausente. |
| `get_value()`, `get_next()`, `get_head()` | `nodeValue()`, `nodeNext()`, `linkedListHead()` | Grain usa funciones puras en lugar de métodos. Los nombres siguen la convención `camelCase` sin prefijos `get_`. |
| `set_next(next)` | `nodeWithNext(node: Node, next: Node)` | Grain es inmutable; no se puede mutar un nodo. `nodeWithNext` devuelve un nodo nuevo con el enlace actualizado. |
| `delete(value)` | `linkedListDelete(list: LinkedList, value: Number)` que devuelve `(Bool, LinkedList)` | Grain es inmutable; la función devuelve una tupla con el éxito y la lista resultante, no muta la instancia recibida. |
| `pop()`, `dequeue()` | `stackPop(stack: Stack)` y `queueDequeue(queue: Queue)` que devuelven `(Number, Stack/Queue)` | Grain es inmutable; las funciones devuelven una tupla con el valor extraído y la estructura resultante. |
| `insert_tail(value)` y `enqueue(value)` con complejidad `O(1)` | Implementación con complejidad `O(n)` | Grain es inmutable; para insertar al final, se reconstruye el camino hasta la cola. No hay mutación en el sitio. |
| Entrada nula o inválida | No se modela explícitamente | La especificación no exige manejar entradas nulas para este módulo; las estructuras se inicializan con las funciones `init`. |

---

## 🚨 Indicadores de fallo / Failure indicators

| Operación / Operation | Situación de fallo / Failure situation | Indicador / Indicator | Ejemplo / Example |
|---|---|---|---|
| `linkedListHead()` | Lista vacía | `-1` | `linkedListHead(list)` devuelve `-1` cuando `linkedListIsEmpty(list)` es `true`. |
| `stackPop()` | Pila vacía | `(-1, misma pila)` | `stackPop(stack)` devuelve `(-1, stack)` cuando `stackIsEmpty(stack)` es `true`. |
| `stackPeek()` | Pila vacía | `-1` | `stackPeek(stack)` devuelve `-1` cuando `stackIsEmpty(stack)` es `true`. |
| `queueDequeue()` | Cola vacía | `(-1, misma cola)` | `queueDequeue(queue)` devuelve `(-1, queue)` cuando `queueIsEmpty(queue)` es `true`. |
| `queuePeek()` | Cola vacía | `-1` | `queuePeek(queue)` devuelve `-1` cuando `queueIsEmpty(queue)` es `true`. |
| `linkedListDelete()` | Valor no está en la lista | `(false, misma lista)` | `linkedListDelete(list, 99)` devuelve `(false, list)` si `99` no existe en la lista. |
| `nodeNext()` | Enlace ausente | `None` | `nodeNext(node)` devuelve `None` cuando el nodo no tiene enlace. |

**ES:** El indicador de fallo es `-1` para operaciones que devuelven `Number`, `None` para operaciones que devuelven `Option<Node>`, y `false` para operaciones que devuelven `Bool`. Grain tiene tipos algebraicos; `Option` con `Some(value)` y `None` es la representación idiomática de ausencia.

**EN:** The failure indicator is `-1` for operations returning `Number`, `None` for operations returning `Option<Node>`, and `false` for operations returning `Bool`. Grain has algebraic types; `Option` with `Some(value)` and `None` is the idiomatic representation of absence.

---

## ✅ Cobertura de pruebas / Test coverage

| Caso de la especificación / Specification case | Cubierto / Covered | Prueba / Test | Notas / Notes |
|---|---|:--:|---|
| **Node**: Inicializar y observar valor/enlace | Sí | `Node — caso 1` | Crea `a` con `nodeInit(10)`, verifica `nodeValue(a)=10` y `nodeNext(a)=None`. |
| **Node**: Inicializar otro nodo, enlazar y recorrer | Sí | `Node — caso 2` | Crea `b` con `nodeInit(20)`, enlaza `a→b`, verifica `nodeValue(nodeNext(a))=20` y `nodeNext(b)=None`. |
| **LinkedList**: Estado vacío | Sí | `LinkedList — paso 1` | Verifica `linkedListIsEmpty()=true`, `linkedListSize()=0`, `linkedListHead()=-1`. |
| **LinkedList**: Insertar por ambos extremos | Sí | `LinkedList — paso 2` | Inserta `10, 20` al final y `5` al principio; verifica `linkedListSize()=4`, `linkedListHead()=5`. |
| **LinkedList**: Eliminar primera aparición | Sí | `LinkedList — paso 3` | Elimina `10` (primera aparición); verifica `delete()=true`, `linkedListHead()=5`, `linkedListSize()=3`. |
| **LinkedList**: Valor ausente | Sí | `LinkedList — paso 4` | Intenta eliminar `99`; verifica `delete()=false`, `linkedListHead()=5`, `linkedListSize()=3`. |
| **LinkedList**: Vaciar | Sí | `LinkedList — paso 5` | Elimina `5, 20, 10`; verifica `linkedListIsEmpty()=true`, `linkedListSize()=0`, `linkedListHead()=-1`. |
| **Stack**: Estado vacío y extracción fallida | Sí | `Stack — paso 1` | Verifica `stackIsEmpty()=true`, `stackSize()=0`, `stackPeek()=-1`, `stackPop()=-1`. |
| **Stack**: LIFO y `peek` no mutante | Sí | `Stack — paso 2` | Apila `10, 20, 30`; verifica `stackPeek()=30`, `stackSize()=3`. |
| **Stack**: Extracción y reutilización | Sí | `Stack — paso 3` | Desapila `30`, apila `40`, desapila `40, 20, 10`; verifica `stackIsEmpty()=true`, `stackSize()=0`. |
| **Stack**: Vacío tras extracción | Sí | `Stack — paso 4` | Desapila de pila vacía; verifica `stackPop()=-1`, `stackIsEmpty()=true`. |
| **Queue**: Estado vacío y extracción fallida | Sí | `Queue — paso 1` | Verifica `queueIsEmpty()=true`, `queueSize()=0`, `queuePeek()=-1`, `queueDequeue()=-1`. |
| **Queue**: FIFO y `peek` no mutante | Sí | `Queue — paso 2` | Encola `10, 20, 30`; verifica `queuePeek()=10`, `queueSize()=3`. |
| **Queue**: Extracción y reutilización | Sí | `Queue — paso 3` | Desencola `10`, encola `40`, desencola `20, 30, 40`; verifica `queueIsEmpty()=true`, `queueSize()=0`. |
| **Queue**: Vacío tras extracción | Sí | `Queue — paso 4` | Desencola de cola vacía; verifica `queueDequeue()=-1`, `queueIsEmpty()=true`. |

**Total de pruebas:** 15 casos con 51 aserciones (2 para `Node`, 17 para `LinkedList`, 15 para `Stack`, 17 para `Queue`).

---

## ⚠️ Limitaciones conocidas / Known limitations

| Limitación / Limitation | Impacto / Impact | Alternativa o plan / Workaround or plan |
|---|---|---|
| `linkedListInsertTail` y `queueEnqueue` tienen complejidad `O(n)` en lugar de `O(1)` | Inserciones al final más lentas en listas grandes | Grain es inmutable; no hay mutación en el sitio. Para mantener la inmutabilidad, se reconstruye el camino hasta la cola. |
| No hay framework de testing estándar | Se usa un framework personalizado `testing.gr` | El framework personalizado proporciona `runTests` y `printSummary` con acumulación de resultados y resumen final. |

---

## 📝 Notas de implementación / Implementation Notes

**ES:** Grain es un lenguaje funcional inmutable con tipos algebraicos y recursión de cola garantizada. Las operaciones no mutan la instancia recibida; cada función devuelve una estructura nueva con los cambios. Esto elimina la necesidad de sincronización o bloqueo, pero requiere reconstruir las estructuras en cada operación.

Los tipos algebraicos `Option<Node>` con `Some(node)` y `None` permiten representar la ausencia de enlace de forma segura, sin `null` ni centinelas. Las tuplas `(Bool, LinkedList)` y `(Number, Stack/Queue)` permiten devolver el valor y la estructura resultante en una sola operación, conservando la inmutabilidad.

La recursión de cola se usa en `relink`, `lastNode`, `collectToTail` y `takePrefix` para evitar desbordamientos de pila en listas grandes. Grain garantiza la optimización de recursión de cola (TCO), así que estas funciones son seguras para listas de cualquier tamaño.

La separación de `src/` y `tests/` con un framework personalizado `testing.gr` permite ejecutar las pruebas con `make test` y obtener un resumen claro con los casos pasados, fallidos y total.

**EN:** Grain is an immutable functional language with algebraic types and guaranteed tail-call optimization. Operations do not mutate the received instance; each function returns a new structure with the changes. This eliminates the need for synchronization or locking, but requires rebuilding structures on each operation.

Algebraic types `Option<Node>` with `Some(node)` and `None` allow representing link absence safely, without `null` or sentinels. Tuples `(Bool, LinkedList)` and `(Number, Stack/Queue)` allow returning the value and the resulting structure in a single operation, preserving immutability.

Tail recursion is used in `relink`, `lastNode`, `collectToTail` and `takePrefix` to avoid stack overflows in large lists. Grain guarantees tail-call optimization (TCO), so these functions are safe for lists of any size.

The separation of `src/` and `tests/` with a custom framework `testing.gr` allows running tests with `make test` and getting a clear summary with passed, failed and total cases.

**ES:** Este proyecto también está implementado en otros lenguajes. Explora el repositorio principal para consultar las demás versiones.

**EN:** This project is also implemented in other languages. Explore the main repository to see the other versions.

---

## 🔍 Checklist de validación / Validation checklist

- [x] La suite nativa se ejecutó y su salida real está copiada en este README.
- [x] Cada caso de la especificación tiene su fila en _Cobertura de pruebas_ (o `Omitido` con razón).
- [x] Cada desviación del pseudocódigo o de la ubicación esperada está en _Adaptaciones idiomáticas_.
- [x] Cada operación con fallo posible está en _Indicadores de fallo_.
- [x] No hay rutas absolutas del autor, credenciales ni salidas inventadas.
- [x] Los enlaces relativos resuelven dentro del repositorio y el documento es bilingüe.
- [x] Ninguna sección repite lo que ya dice la especificación.

---

*[← Volver a Algorithms](../README.md)*

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
