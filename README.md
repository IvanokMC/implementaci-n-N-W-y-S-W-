# 🧬 GenomX Visualizer — Bioinformática

Visualizador interactivo de algoritmos fundamentales de bioinformática, implementado en HTML5 + CSS3 + JavaScript puro (sin dependencias externas).

🔗 **Demo en vivo:** [Ver en GitHub Pages](https://ivanokmc.github.io/implementaci-n-N-W-y-S-W-/)

---

## 📋 Algoritmos implementados

| Módulo | Algoritmo | Tipo |
|--------|-----------|------|
| 🔴 Smith-Waterman | Programación dinámica | Alineamiento **local** |
| 🟢 Needleman-Wunsch | Programación dinámica | Alineamiento **global** |
| 🟡 BWT + FM-Index | Transformada + Backward Search | Búsqueda exacta |

---

## 🚀 Uso

### Opción 1 — Abrir localmente
```bash
# Clonar el repositorio
git clone https://github.com/TU-USUARIO/genomx-visualizer.git
cd genomx-visualizer

# Abrir directamente en el navegador (sin servidor necesario)
open genomx_visualizer.html        # macOS
start genomx_visualizer.html       # Windows
xdg-open genomx_visualizer.html    # Linux
```

### Opción 2 — GitHub Pages (recomendado)
1. Ir a **Settings → Pages** en el repositorio
2. Source: **Deploy from a branch → main → / (root)**
3. Acceder a `https://TU-USUARIO.github.io/genomx-visualizer`

---

## 🧠 Descripción de los algoritmos

### 1. Smith-Waterman (Alineamiento Local)
Encuentra la **región más similar** entre dos secuencias. La diferencia clave con Needleman-Wunsch es que los valores nunca bajan de 0 (reinicio), permitiendo identificar regiones homólogas ignorando las extremidades.

**Fórmula de recurrencia:**
```
H(i,j) = max(0, H(i-1,j-1)+s(i,j), H(i-1,j)−gap, H(i,j-1)−gap)
```

- El traceback inicia en **todas las celdas con valor máximo global**
- Se visualizan **múltiples rutas óptimas** simultáneamente con colores distintos
- Puntuación: Match **+1** | Mismatch **−1** | Gap **−1**

---

### 2. Needleman-Wunsch (Alineamiento Global)
Alinea las dos secuencias **de extremo a extremo**. Los bordes se inicializan con penalizaciones acumuladas de gap, y el traceback siempre comienza en la esquina inferior derecha **F(m,n)**.

**Fórmula de recurrencia:**
```
F(i,j) = max(F(i-1,j-1)+s(i,j), F(i-1,j)−gap, F(i,j-1)−gap)
```

- Puntuación: Match **+1** | Mismatch **−1** | Gap **−1**
- Los valores **sí pueden ser negativos** (a diferencia de SW)

---

### 3. Burrows-Wheeler Transform (BWT) + FM-Index

**3 fases visualizadas paso a paso:**

| Fase | Descripción |
|------|-------------|
| **Rotaciones** | Se generan N rotaciones cíclicas del texto T+'$' |
| **Ordenamiento** | Las rotaciones se ordenan lexicográficamente |
| **Extracción BWT** | La última columna = BWT. La primera columna = F |

**FM-Index (Backward Search):**
- Tabla **C[c]**: número de caracteres menores que c en T
- Tabla **OCC(c,i)**: ocurrencias acumuladas de c en BWT[0..i]
- Búsqueda del patrón P leyendo de **derecha a izquierda**

```
top = C[c] + OCC(c, top-1)
bot = C[c] + OCC(c, bot) - 1
```

Las posiciones de ocurrencias se obtienen del **Suffix Array** en el rango `[top..bot]`.

---

## 🎮 Controles de la interfaz

| Botón | Función |
|-------|---------|
| **Paso a Paso** | Calcula una sola celda / fase por clic |
| **Completar Todo** | Termina el cálculo instantáneamente |
| **Reiniciar** | Vuelve al estado inicial con los inputs actuales |

---

## 📁 Estructura del repositorio

```
genomx-visualizer/
├── index.html          # Aplicación (versión limpia para ejecución)
├── genomx_visualizer_comentado.html # Código fuente con comentarios exhaustivos
└── README.md                        # Este archivo
```

---

## 🛠️ Tecnologías

- **HTML5** — Estructura y contenido
- **CSS3** — Estilos, animaciones y tema Tokyo Night
- **JavaScript ES6+** — Algoritmos y lógica (módulos IIFE, arrow functions, spread operator)
- **Sin dependencias externas** — Funciona offline, sin npm, sin bundler

---

## 📸 Capturas de pantalla

### Smith-Waterman — Alineamiento Local
La matriz se llena paso a paso. Las rutas óptimas se pintan con colores distintos.

### Needleman-Wunsch — Alineamiento Global
Borde inicializado con penalizaciones. El traceback traza el camino óptimo en rojo.

### BWT + FM-Index
Visualización de las 3 fases + dashboard con tablas C, OCC y resultado del Backward Search.

---

## 📖 Referencias

- Smith, T.F. & Waterman, M.S. (1981). *Identification of common molecular subsequences*. Journal of Molecular Biology.
- Needleman, S.B. & Wunsch, C.D. (1970). *A general method applicable to the search for similarities in the amino acid sequence of two proteins*. Journal of Molecular Biology.
- Burrows, M. & Wheeler, D.J. (1994). *A block-sorting lossless data compression algorithm*. Digital Equipment Corporation.
- Ferragina, P. & Manzini, G. (2000). *Opportunistic data structures with applications*. FOCS.

---

## 📄 Licencia

Proyecto académico — Bioinformática. Libre uso educativo.
