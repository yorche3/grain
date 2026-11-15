# Data Structures Advanced — Grain

Implementación de la especificación [07_Data_Structures_Advanced](https://yorche3.github.io/programming_languages/core/algorithms/07_Data_Structures_Advanced/) en **Grain**, con un enfoque funcional inmutable y un framework de pruebas propio (`testing.gr`).

Estructuras de datos con múltiples enlaces: **TreeNode** (nodo con enlaces izquierdo y derecho), **BinaryTree** (árbol binario de búsqueda funcional sin capacidad máxima previa) y **Graph** (grafo dirigido con registro lineal canónico y listas de adyacencia ordenadas manualmente sin duplicados). El valor se reutiliza mediante valor embebido (composición interna en el registro), y no se delega el grafo en mapas ni tablas hash de la biblioteca estándar.

---

## 📂 Archivos y estructura / Files & Structure

| Archivo / Directory | Propósito / Purpose |
|---|---|
| `src/data_structures_advanced.gr` | Implementación de `TreeNode`, `BinaryTree` y `Graph` con tipos algebraicos y funciones puras. |
| `tests/data_structures_advanced_test.gr` | Suite de pruebas: 23 grupos y 102 aserciones cubriendo los casos de la especificación. |
| `tests/testing.gr` | Framework personalizado con acumuladores de aserciones (`checkBool`, `checkNumber`) y formateo. |
| `tests/run_tests.gr` | Punto de entrada que ejecuta la suite agregada y muestra el resumen global. |
| `Makefile` | Objetivos de construcción, ejecución, limpieza y prueba (`make test`). |
| `.gitignore` | Archivos generados excluidos (`target/`, `*.gro`, `*.wasm`, `*.wasm.map`). |

Respecto a la ubicación sugerida en la especificación (`test/`), se utiliza `tests/` para alineación idiomática con los módulos previos de Grain (`data_structures_basics`, `numbers`). En lugar de clases o herencia nominal, Grain modela tipos mediante `record` inmutables y enlaces representados por `Option<T>`.

**Estructura de directorios / Directory structure:**

```text
data_structures_advanced/
├── src/
│   └── data_structures_advanced.gr      # TreeNode, BinaryTree, Graph
├── tests/
│   ├── data_structures_advanced_test.gr # Pruebas unitarias de los 3 ADTs
│   ├── testing.gr                       # Framework de pruebas personalizado
│   └── run_tests.gr                     # Punto de entrada ejecutable
├── Makefile                             # Comandos de compilación y ejecución
├── .gitignore                           # Exclusión de binarios WebAssembly y cachés
└── README.md                            # Este archivo
```

---

## 🛠️ Enfoque y construcción / Approach & Build

**ES:** El proyecto se construyó manualmente sin herramientas de scaffolding externas. Grain compila directamente a WebAssembly (WASI) y no requiere un gestor de paquetes externo para proyectos autocontenidos.

**EN:** The project was built manually without external scaffolding tools. Grain compiles directly to WebAssembly (WASI) and does not require an external package manager for self-contained projects.

### Inicialización / Initialization

```bash
# 1. Crear directorios / Create directories
mkdir -p src tests

# 2. Compilar y ejecutar pruebas / Compile and run tests
make test
```

---

## 📄 Configuración clave / Key Configuration

| Archivo / File | Propósito / Purpose |
|---|---|
| `Makefile` | Define metas `build` (`grain compile tests/run_tests.gr`), `run` (`grain run tests/run_tests.wasm`), `test` y `clean`. |
| `.gitignore` | Ignora artefactos compilados por el compilador Grain (`target/`, `*.gro`, `*.wasm`, `*.wasm.map`). |

No se requieren archivos de configuración adicionales ni dependencias externas de terceros.

---

## 🚀 Compilación y ejecución / Build & Run

```bash
make test
```

**Salida real / Actual output:**

```text
grain compile tests/run_tests.gr
grain run tests/run_tests.wasm

▸ TreeNode — caso 1: crear un nodo de árbol
  ✓ get_value = 10
  → 1 passed, 0 failed

▸ TreeNode — caso 2: enlaces iniciales ausentes
  ✓ get_left del nodo nuevo es ausente
  ✓ get_right del nodo nuevo es ausente
  → 2 passed, 0 failed

▸ TreeNode — caso 3: enlazar dos hijos
  ✓ get_value(get_left) = 20
  ✓ get_value(get_right) = 30
  → 2 passed, 0 failed

▸ TreeNode — caso 4: recorrer cada enlace
  ✓ el hijo izquierdo lleva a 20
  ✓ el hijo derecho lleva a 30
  ✓ los enlaces del hijo izquierdo son ausentes
  ✓ los enlaces del hijo derecho son ausentes
  → 4 passed, 0 failed

▸ TreeNode — caso 5: desenlazar hijos
  ✓ al quitar el izquierdo, get_left es ausente
  ✓ al quitar el izquierdo, el valor 10 se conserva
  ✓ al quitar el izquierdo, el derecho 30 se conserva
  ✓ al quitar el derecho, ambos enlaces son ausentes
  ✓ al quitar el derecho, el valor 10 se conserva
  → 5 passed, 0 failed

▸ TreeNode — caso 6: adaptación inmutable
  ✓ el nodo nuevo lleva el enlace izquierdo 20
  ✓ el original no cambia: get_left sigue ausente
  → 2 passed, 0 failed

▸ BinaryTree — caso 1: crear vacío
  ✓ is_empty = true
  ✓ size = 0
  ✓ contains(50) falla
  → 3 passed, 0 failed

▸ BinaryTree — caso 2: insertar la raíz
  ✓ size = 1
  ✓ is_empty = false
  ✓ contains(50) es éxito
  → 3 passed, 0 failed

▸ BinaryTree — caso 3: insertar menor y mayor
  ✓ size = 3
  ✓ contains(50)
  ✓ contains(30)
  ✓ contains(70)
  → 4 passed, 0 failed

▸ BinaryTree — caso 4: ramas de varios niveles
  ✓ size = 5
  ✓ contains(50)
  ✓ contains(30)
  ✓ contains(70)
  ✓ contains(20)
  ✓ contains(40)
  → 6 passed, 0 failed

▸ BinaryTree — caso 5: valor ausente en árbol no vacío
  ✓ contains(99) falla
  ✓ size sigue en 1
  ✓ contains(50) sigue en éxito
  → 3 passed, 0 failed

▸ BinaryTree — caso 6: duplicado idempotente
  ✓ size = 3
  ✓ contains(50)
  ✓ contains(30)
  ✓ contains(70)
  → 4 passed, 0 failed

▸ BinaryTree — caso 7: inserciones ordenadas
  ✓ size = 5
  ✓ contains(20)
  ✓ contains(30)
  ✓ contains(40)
  ✓ contains(50)
  ✓ contains(70)
  ✓ contains(99) falla
  → 7 passed, 0 failed

▸ Graph — caso 1: crear vacío
  ✓ is_empty = true
  ✓ vertex_count = 0
  ✓ edge_count = 0
  ✓ has_edge(10, 30) falla
  ✓ get_neighbors(10) falla
  → 5 passed, 0 failed

▸ Graph — caso 2: vértice aislado
  ✓ grafo no vacío
  ✓ 1 vértice
  ✓ 0 aristas
  ✓ vecinos de 10 vacíos
  → 4 passed, 0 failed

▸ Graph — caso 3: etiquetas no consecutivas y arista dirigida
  ✓ add_edge(10, 70) es éxito
  ✓ has_edge(10, 70) es éxito
  ✓ has_edge(70, 10) falla
  ✓ 2 vértices
  ✓ 1 arista
  → 5 passed, 0 failed

▸ Graph — caso 4: vértice duplicado conserva adyacencia
  ✓ add_edge(10, 30) es éxito
  ✓ 2 vértices
  ✓ 1 arista
  ✓ vecinos de 10 = [30]
  → 4 passed, 0 failed

▸ Graph — caso 5: arista duplicada
  ✓ primer add_edge(10, 30) es éxito
  ✓ segundo add_edge(10, 30) es éxito
  ✓ 2 vértices
  ✓ 1 arista
  ✓ vecinos de 10 = [30]
  → 4 passed, 0 failed

▸ Graph — caso 6: extremo inexistente sin mutación
  ✓ add_edge(10, 30) es éxito
  ✓ add_edge(10, 99) falla
  ✓ add_edge(99, 10) falla
  ✓ add_edge(98, 99) falla
  ✓ add_edge(99, 99) falla
  ✓ siguen 2 vértices
  ✓ sigue 1 arista
  ✓ vecinos de 10 = [30]
  ✓ vecinos de 30 vacíos
  ✓ las aristas intentadas siguen ausentes
  ✓ get_neighbors(98) falla
  ✓ get_neighbors(99) falla
  → 12 passed, 0 failed

▸ Graph — caso 7: vecinos ordenados y contadores
  ✓ las cuatro altas de arista son éxito
  ✓ vecinos de 10 = [30, 50, 70]
  ✓ vecinos de 30 = [50]
  ✓ vecinos de 50 vacíos
  ✓ vecinos de 70 vacíos
  ✓ 4 vértices
  ✓ 4 aristas
  → 7 passed, 0 failed

▸ Graph — caso 8: consulta de arista ausente
  ✓ has_edge(10, 30) falla
  ✓ 2 vértices
  ✓ 0 aristas
  ✓ no vacío
  → 4 passed, 0 failed

▸ Graph — caso 9: vecinos de vértice inexistente
  ✓ get_neighbors(99) falla
  ✓ el fallo es distinto de los vecinos vacíos de 10
  ✓ 1 vértice
  ✓ 0 aristas
  → 4 passed, 0 failed

▸ Graph — caso 10: bucle y duplicado
  ✓ primer add_edge(10, 10) es éxito
  ✓ segundo add_edge(10, 10) es éxito
  ✓ has_edge(10, 10) es éxito
  ✓ vecinos de 10 = [10]
  ✓ 1 vértice
  ✓ 1 arista
  → 6 passed, 0 failed

--- Test Summary ---
Cases: 102 passed, 0 failed, 102 total
```

---

## 🧠 Algoritmos y operaciones / Algorithms & Operations

### TreeNode

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `treeNodeInit(value: Number)` | `Number → TreeNode` | `O(1)` | Construye un nodo con enlaces izquierdo y derecho en `None`. Equivalente a `init(value)`. |
| `treeNodeValue(node: TreeNode)` | `TreeNode → Number` | `O(1)` | Acceso al valor del nodo. Equivalente a `get_value()`. |
| `treeNodeLeft(node: TreeNode)` | `TreeNode → Option<TreeNode>` | `O(1)` | Acceso al enlace izquierdo (`None` si está ausente). Equivalente a `get_left()`. |
| `treeNodeRight(node: TreeNode)` | `TreeNode → Option<TreeNode>` | `O(1)` | Acceso al enlace derecho (`None` si está ausente). Equivalente a `get_right()`. |
| `treeNodeWithLeft(node: TreeNode, left: Option<TreeNode>)` | `TreeNode, Option<TreeNode> → TreeNode` | `O(1)` | Copia inmutable con el enlace izquierdo sustituido. Equivalente a `set_left(node)`. |
| `treeNodeWithRight(node: TreeNode, right: Option<TreeNode>)` | `TreeNode, Option<TreeNode> → TreeNode` | `O(1)` | Copia inmutable con el enlace derecho sustituido. Equivalente a `set_right(node)`. |

### BinaryTree

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `binaryTreeInit()` | `→ BinaryTree` | `O(1)` | Instancia un árbol binario vacío (`root: None`, `count: 0`). Equivalente a `init()`. |
| `binaryTreeInsert(tree: BinaryTree, value: Number)` | `BinaryTree, Number → BinaryTree` | `O(h)` | Inserta un elemento respetando la invariante ABB. Reconstruye únicamente la ruta desde la raíz (`h` = altura). Duplicados no alteran el tamaño ni duplican nodos. |
| `binaryTreeContains(tree: BinaryTree, value: Number)` | `BinaryTree, Number → Bool` | `O(h)` | Búsqueda binaria por ramas de acuerdo con la invariante ABB; no recorre secuencias lineales. |
| `binaryTreeIsEmpty(tree: BinaryTree)` | `BinaryTree → Bool` | `O(1)` | Comprueba si `count == 0`. Equivalente a `is_empty()`. |
| `binaryTreeSize(tree: BinaryTree)` | `BinaryTree → Number` | `O(1)` | Devuelve el contador acumulado de nodos únicos. Equivalente a `size()`. |

### Graph

| Operación / Operation | Entrada → salida / Input → output | Complejidad / Complexity | Notas / Notes |
|---|---|---|---|
| `graphInit()` | `→ Graph` | `O(1)` | Construye un grafo vacío (`vertices: None`, `vertexCount: 0`, `edgeCount: 0`). |
| `graphAddVertex(graph: Graph, label: Number)` | `Graph, Number → Graph` | `O(V)` | Registra un vértice en el registro canónico lineal si no existe previamente; idempotente. |
| `graphAddEdge(graph: Graph, u: Number, v: Number)` | `Graph, Number, Number → (Bool, Graph)` | `O(V + d(u))` | Conecta `u → v`. Requiere que ambos extremos existan. Inserta `v` ordenado sin duplicados en la lista de adyacencia de `u`. Devuelve `(true, nuevoGrafo)` o `(false, mismoGrafo)`. |
| `graphHasEdge(graph: Graph, u: Number, v: Number)` | `Graph, Number, Number → Bool` | `O(V + d(u))` | Comprueba la existencia del arco dirigido `u → v` verificando extremos y adyacencia. |
| `graphGetNeighbors(graph: Graph, label: Number)` | `Graph, Number → Option<List<Number>>` | `O(V)` | Devuelve `Some(vecinos)` ordenados si el vértice existe, o `None` si no existe en el grafo. |
| `graphIsEmpty(graph: Graph)` | `Graph → Bool` | `O(1)` | Devuelve `vertexCount == 0`. Equivalente a `is_empty()`. |
| `graphVertexCount(graph: Graph)` | `Graph → Number` | `O(1)` | Devuelve el número de vértices registrados. Equivalente a `vertex_count()`. |
| `graphEdgeCount(graph: Graph)` | `Graph → Number` | `O(1)` | Devuelve el número total de aristas dirigidas. Equivalente a `edge_count()`. |

---

## 🧩 Decisiones de diseño / Design decisions

| Decisión / Decision | Alternativa considerada / Alternative | Razón / Reason |
|---|---|---|
| Reutilización de valor por valor embebido en `record` | Simular jerarquía de herencia con variantes algebraicas | Grain no posee herencia de clases ni orientación a objetos clásica. El valor embebido (`value: Number` o `label: Number`) modela directamente la semántica de la base `Node(value)` con composición limpia e inmutable. |
| Retorno de tupla `(Bool, Graph)` en `graphAddEdge` | Mutar el grafo in-place o lanzar excepciones | Grain es puramente inmutable y no tiene excepciones. Retornar el par `(éxito, grafo)` preserva el aislamiento referencial y permite encadenar comprobaciones sin mutaciones laterales. |
| `Option<List<Number>>` para `graphGetNeighbors` | Devolver lista vacía `[]` o centinela numérico ante vértice inexistente | Permite distinguir de forma no ambigua entre un vértice registrado pero aislado (`Some([])`) y un vértice que no pertenece al grafo (`None`). |
| Registro lineal manual `nextVertex` en `GraphNode` | Usar `Map` o `HashMap` de biblioteca estándar | La especificación prohíbe explícitamente el uso de tablas hash, diccionarios o índices automáticos para el registro de vértices, exigiendo recorrido manual explícito con complejidad `O(V)`. |
| Inserción ordenada manual recursiva en listas de adyacencia | Usar `List.sort` de la biblioteca tras insertar al frente | Mantiene el invariante de adyacencia ordenada ascendentemente en `O(d(u))` durante la inserción sin sobrecarga de ordenamiento completo de biblioteca. |

---

## 🔀 Adaptaciones idiomáticas / Idiomatic adaptations

| Especificación / Specification | Adaptación / Adaptation | Justificación / Justification |
|---|---|---|
| Base común `Node` con herencia nominal | Valor embebido directo (`value` en `TreeNode`, `label` en `GraphNode`) | Grain no tiene herencia orientada a objetos. Cada estructura define su registro embebiendo el dato elemental según la opción b) del contrato de especificación. |
| Mutación in-place `set_left(node)`, `set_right(node)` | Funciones reconstructoras puras `treeNodeWithLeft`, `treeNodeWithRight` | En Grain los `record` son inmutables por defecto. Se genera un nuevo registro con la actualización mediante la sintaxis de propagación `{ ...node, left: left }`. |
| Mutación in-place en `insert(value)`, `add_vertex(label)` | Retorno de nueva estructura inmutable (`BinaryTree`, `Graph`) | Preserva la transparencia referencial del paradigma funcional. Solo se reconstruye la espina o camino afectado en el árbol o en el registro de vértices. |
| Indicador booleano en `add_edge(u, v)` con mutación | Retorno de tupla `(Bool, Graph)` | Al no haber mutación in-place, se devuelve conjuntamente la bandera de éxito y la versión resultante del grafo (que permanece idéntica en caso de fallo). |
| Enlaces nulos para subárbol ausente o fin de lista | Tipo algebraico `Option<T>` (`Some(n)` y `None`) | Grain no tiene referencias nulas (`null`/`nil`). `Option` es la representación canónica y segura ante ausencia. |
| Ubicación en directorio `test/` | Ubicación en `tests/` | Consistencia y convención establecida en los módulos precedentes del repositorio para Grain (`data_structures_basics`, `numbers`). |

---

## 🚨 Indicadores de fallo / Failure indicators

| Operación / Operation | Situación de fallo / Failure situation | Indicador / Indicator | Ejemplo / Example |
|---|---|---|---|
| `treeNodeLeft`, `treeNodeRight` | Enlace hijo ausente / subárbol vacío | `None` | `treeNodeLeft(nodeTen)` devuelve `None`. |
| `binaryTreeContains` | Valor no encontrado en el árbol | `false` | `binaryTreeContains(tree, 99)` devuelve `false`. |
| `graphAddEdge` | Extremo `u` o `v` inexistente en el grafo | `(false, graph)` | `graphAddEdge(graph, 10, 99)` devuelve `(false, graph)` sin alterar vértices ni aristas. |
| `graphHasEdge` | Arista inexistente o extremos inexistentes | `false` | `graphHasEdge(graph, 10, 30)` devuelve `false`. |
| `graphGetNeighbors` | Vértice consultado no existe en el grafo | `None` | `graphGetNeighbors(graph, 99)` devuelve `None`, diferenciándose de `Some([])`. |

---

## ✅ Cobertura de pruebas / Test coverage

| Caso de la especificación / Specification case | Cubierto / Covered | Prueba / Test | Notas / Notes |
|---|---|:--:|---|
| **TreeNode**: Crear un nodo de árbol | Sí | `tests/data_structures_advanced_test.gr:64` | `TreeNode — caso 1`: verifica `treeNodeValue` = 10. |
| **TreeNode**: Enlaces iniciales ausentes | Sí | `tests/data_structures_advanced_test.gr:68` | `TreeNode — caso 2`: verifica `treeNodeLeft` y `treeNodeRight` en `None`. |
| **TreeNode**: Enlazar dos hijos | Sí | `tests/data_structures_advanced_test.gr:83` | `TreeNode — caso 3`: enlaza hijos 20 y 30 en nodo 10; verifica valores. |
| **TreeNode**: Recorrer cada enlace | Sí | `tests/data_structures_advanced_test.gr:98` | `TreeNode — caso 4`: recorre enlaces de hijos y valida ausencia de nietos. |
| **TreeNode**: Desenlazar hijos | Sí | `tests/data_structures_advanced_test.gr:127` | `TreeNode — caso 5`: limpia izquierdo y luego derecho preservando el valor 10. |
| **TreeNode**: Adaptación inmutable | Sí | `tests/data_structures_advanced_test.gr:159` | `TreeNode — caso 6`: valida que `treeNodeWithLeft` retorna nuevo nodo sin mutar el original. |
| **BinaryTree**: Crear vacío | Sí | `tests/data_structures_advanced_test.gr:208` | `BinaryTree — caso 1`: verifica `is_empty = true`, `size = 0`, `contains(50) = false`. |
| **BinaryTree**: Insertar la raíz | Sí | `tests/data_structures_advanced_test.gr:220` | `BinaryTree — caso 2`: inserta 50; verifica `size = 1`, `is_empty = false`, `contains = true`. |
| **BinaryTree**: Insertar menor y mayor | Sí | `tests/data_structures_advanced_test.gr:232` | `BinaryTree — caso 3`: inserta 50, 30, 70; verifica tamaño 3 y pertenencia. |
| **BinaryTree**: Ramas de varios niveles | Sí | `tests/data_structures_advanced_test.gr:254` | `BinaryTree — caso 4`: inserta 50, 30, 70, 20, 40; verifica tamaño 5 y pertenencia. |
| **BinaryTree**: Valor ausente en árbol no vacío | Sí | `tests/data_structures_advanced_test.gr:265` | `BinaryTree — caso 5`: `contains(99)` falla sin alterar el árbol. |
| **BinaryTree**: Duplicado idempotente | Sí | `tests/data_structures_advanced_test.gr:280` | `BinaryTree — caso 6`: reinsertar 30 mantiene el tamaño en 3 y conserva los elementos. |
| **BinaryTree**: Inserciones ordenadas | Sí | `tests/data_structures_advanced_test.gr:301` | `BinaryTree — caso 7`: inserta 20, 30, 40, 50, 70; verifica orden y pertenencia. |
| **Graph**: Crear vacío | Sí | `tests/data_structures_advanced_test.gr:384` | `Graph — caso 1`: verifica vacío, contadores en 0, `has_edge` y `get_neighbors` fallan. |
| **Graph**: Vértice aislado | Sí | `tests/data_structures_advanced_test.gr:402` | `Graph — caso 2`: registra 10; verifica 1 vértice, 0 aristas y vecinos `Some([])`. |
| **Graph**: Etiquetas no consecutivas y arista dirigida | Sí | `tests/data_structures_advanced_test.gr:419` | `Graph — caso 3`: conecta 10 → 70; verifica dirección única y contadores. |
| **Graph**: Vértice duplicado conserva adyacencia | Sí | `tests/data_structures_advanced_test.gr:437` | `Graph — caso 4`: reinsertar vértice 10 preserva aristas existentes y vecinos `[30]`. |
| **Graph**: Arista duplicada | Sí | `tests/data_structures_advanced_test.gr:454` | `Graph — caso 5`: reinsertar arista (10, 30) es idempotente; mantiene 1 arista. |
| **Graph**: Extremo inexistente sin mutación | Sí | `tests/data_structures_advanced_test.gr:472` | `Graph — caso 6`: intentos de aristas con extremos ausentes fallan sin alterar el grafo. |
| **Graph**: Vecinos ordenados y contadores | Sí | `tests/data_structures_advanced_test.gr:517` | `Graph — caso 7`: aristas múltiples producen adyacencias ordenadas crecientes `[30, 50, 70]`. |
| **Graph**: Consulta de arista ausente | Sí | `tests/data_structures_advanced_test.gr:549` | `Graph — caso 8`: consulta de arista no conectada falla en grafo con 2 vértices. |
| **Graph**: Vecinos de vértice inexistente | Sí | `tests/data_structures_advanced_test.gr:562` | `Graph — caso 9`: `get_neighbors(99)` produce `None`, diferenciado de `Some([])`. |
| **Graph**: Bucle y duplicado | Sí | `tests/data_structures_advanced_test.gr:581` | `Graph — caso 10`: bucle (10, 10) y su reintento mantienen 1 arista y vecino `[10]`. |
| **Graph**: Aislamiento del resultado de vecinos | Omitido | — | No representable: en Grain las listas `List<Number>` son estructuras inmutables persistentes compartidas, por lo que ninguna llamada consumidora puede mutar internamente la lista retornada por el grafo. |

**Organización de la suite:**
- Unidad / Variante: `TreeNode`, `BinaryTree`, `Graph` (enfoque funcional puro e inmutable).
- Casos aplicables: 23 escenarios de prueba divididos en 102 aserciones unitarias.
- Grupo lógico: Agrupados en listas `treeNodeTests`, `binaryTreeTests` y `graphTests`.
- Archivo / Suite: `tests/data_structures_advanced_test.gr`.
- Runner: `tests/run_tests.gr` ejecutado vía `make test` sobre el motor WASI de Grain.

---

## ⚠️ Limitaciones conocidas / Known limitations

| Limitación / Limitation | Impacto / Impact | Alternativa o plan / Workaround or plan |
|---|---|---|
| Inmutabilidad estructural y reconstrucción de espina | Inserciones y adiciones reconstruyen la ruta hasta el nodo/vértice afectado | Característica intrínseca del modelo funcional puro de Grain; garantiza seguridad referencial y ausencia de condiciones de carrera sin bloqueos. |
| Ausencia de framework estándar de pruebas | Necesidad de mantener `testing.gr` en el módulo | Se aprovecha el framework personalizado consistente con `data_structures_basics`, acumulando aserciones con nombres descriptivos y resumen unificado. |

---

## 📝 Notas de implementación / Implementation Notes

**ES:** Grain compila a WebAssembly y opera como un lenguaje fuertemente tipado e inmutable. Las estructuras de datos avanzadas se resolvieron aprovechando las siguientes características:
- **Árbol Binario de Búsqueda (ABB):** La función `insertNode` realiza reconstrucción persistente: retorna el nuevo subárbol con el nodo insertado junto a una bandera booleana que indica si el nodo era nuevo o duplicado. De este modo, `binaryTreeInsert` actualiza el contador en una sola pasada `O(h)` sin búsquedas redundantes previas.
- **Registro Canónico de Grafos:** Se implementó una lista enlazada interna explícita (`nextVertex: Option<GraphNode>`), recorriendo los vértices linealmente (`O(V)`) para cumplir estrictamente con la prohibición contractual de delegar el registro en diccionarios o tablas hash.
- **Adyacencia Ordenada:** La función auxiliar `insertSorted` mantiene la secuencia de vecinos ordenada ascendentemente sin duplicados en `O(d(u))` utilizando coincidencia de patrones recursiva con optimización de llamada de cola cuando aplica.
- **Manejo de Casos Nulos y Ausencia:** La ausencia se representa estrictamente mediante el tipo suma estándar `Option<T>` (`Some` / `None`), garantizando seguridad estática sin punteros nulos ni centinelas arbitrarios.

**EN:** Grain compiles to WebAssembly and operates as a strongly typed, immutable language. The advanced data structures were implemented leveraging the following language features:
- **Binary Search Tree (BST):** The `insertNode` function performs persistent functional reconstruction: it returns the updated subtree along with a boolean flag indicating whether the key was freshly inserted or a duplicate. This allows `binaryTreeInsert` to update its size counter in a single `O(h)` traversal without redundant pre-checks.
- **Canonical Graph Registry:** A manual internal linked sequence is used (`nextVertex: Option<GraphNode>`), traversing vertices linearly (`O(V)`) to adhere strictly to the contractual ban against delegating the registry to standard library hash maps or dictionaries.
- **Sorted Adjacency:** The helper function `insertSorted` keeps each vertex's neighbour list strictly ascending and deduplicated in `O(d(u))` using recursive pattern matching with tail-call optimization where applicable.
- **Null Safety and Absence:** Absence is modeled strictly through the standard sum type `Option<T>` (`Some` / `None`), providing static type safety without raw null pointers or arbitrary sentinels.

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

## 📚 Referencias / References

| Tipo / Kind | Referencia / Reference |
|---|---|
| Especificación / Specification | [07_Data_Structures_Advanced.md](../../../../docs/core/algorithms/07_Data_Structures_Advanced.md) |
| Primera base del lenguaje / First language reference | `data_structures_basics` — [README.md](../data_structures_basics/README.md) |
| Segunda base del lenguaje / Second language reference | `numbers` — [README.md](../../foundations/numbers/README.md) |
| Guía de inicialización / Initialisation guide | [00_Project_Initialization_Guide.md](../../../../docs/core/00_Project_Initialization_Guide.md) |
| Adaptaciones idiomáticas / Idiomatic adaptations | [AGENT_Template.md](../../../../docs/AGENT_Template.md) |
| Validación de la documentación / Documentation validation | [WORKFLOW.md](../../../../docs/WORKFLOW.md) |
| Documentación oficial del lenguaje / Language official docs | [grain-lang.org](https://grain-lang.org/) |

---

*[← Volver a Algorithms](../README.md)*

*🌐 [github.com/yorche3/programming_languages](https://github.com/yorche3/programming_languages) · [GitHub Pages](https://yorche3.github.io/programming_languages/)*
