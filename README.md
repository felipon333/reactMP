# Marketplace UNAB — Marketplace Universitario
> **Taller Evaluado 2 · Desarrollo de Frontend con React**  
> **Asignatura:** Desarrollo Web y Móvil · Segundo Semestre 2026  
> **Problemática Seleccionada:** Opción 4 — *Marketplace Universitario*

---

## 👥 Integrantes del Equipo
- **Estudiante(s):** Javier Leiva (Completar con integrantes / carrera / sección)
- **Docente:** (Completar según corresponda)
- **Institución:** Universidad Andrés Bello (UNAB)

---

## 🎯 1. Descripción de la Problemática y Usuarios Objetivo

### Problemática
Dentro de la comunidad universitaria de la UNAB, la compraventa e intercambio de libros académicos, calculadoras, notebooks, apuntes y artículos deportivos suele realizarse de forma desorganizada e informal a través de grupos de redes sociales o mensajería. Esto genera falta de visibilidad, precios arbitrarios y dificultades para encontrar materiales específicos de asignaturas de pregrado.

### Propuesta de Solución
**Marketplace UNAB** es una aplicación frontend desarrollada en **React 18** y **Vite** que centraliza las publicaciones estudiantiles. Permite explorar un catálogo dinámico con filtros avanzados, revisar detalles de los productos y sus vendedores (identificando su carrera y campus), gestionar un carrito de compras y favoritos con persistencia local, publicar nuevos artículos con validaciones en cliente, e integrar la API pública de **Open Library** para enriquecer automáticamente la publicación de libros universitarios.

### Usuarios Objetivo
- Estudiantes de pregrado y postgrado de la UNAB que buscan adquirir material académico a precios accesibles.
- Estudiantes que desean vender libros o accesorios que ya no utilizan al finalizar un semestre.

---

## 🛠️ 2. Tecnologías y Herramientas

| Tecnología / Herramienta | Uso en el Proyecto |
| :--- | :--- |
| **React 18** | Arquitectura basada en componentes funcionales, renderizado declarativo, props y hooks (`useState`, `useEffect`, `useMemo`). |
| **Vite 5** | Entorno de desarrollo rápido, empaquetador moderno y configuración optimizada. |
| **JavaScript (ES6+)** | Lógica funcional, manipulación de arrays (`filter`, `reduce`, `map`, `some`), eventos y consumo asíncrono con `async/await`. |
| **Bootstrap 5.3** | Sistema de grillas responsivas (Grid), componentes visuales (modales, offcanvas, cards, badges) y utilidades. |
| **Bootstrap Icons** | Iconografía vectorial para acciones (carrito, favoritos, búsqueda, eliminación, categorías). |
| **CSS3 Personalizado** | Identidad gráfica institucional UNAB (`#ad0f0f` rojo corporativo y `#810b0b` en hover/active). |
| **Open Library API** | API pública externa para búsqueda bibliográfica y autocompletado inteligente de publicaciones. |
| **LocalStorage** | Simulación de persistencia en el navegador para carrito y favoritos. |
| **Git / GitHub** | Control de versiones, ramas y registro de trazabilidad. |

---

## 🚀 3. Instrucciones de Instalación y Ejecución

### Prerrequisitos
- Node.js versión 18 o superior instalada.
- npm (Node Package Manager).

### Pasos para ejecutar en local

1. Clonar el repositorio y posicionarse en la carpeta del proyecto:
   ```bash
   git clone https://github.com/JavierLeivaL/Marketplace-universitario-Taller-1.git
   cd Marketplace-universitario-Taller-1
   ```

2. Instalar dependencias:
   ```bash
   npm install
   ```

3. Iniciar el servidor de desarrollo con Vite:
   ```bash
   npm run dev
   ```

4. Abrir en el navegador la dirección local indicada en la consola (usualmente `http://localhost:5173` o `http://localhost:3000`).

---

## 📁 4. Arquitectura y Estructura del Código

El proyecto sigue una estricta separación de responsabilidades y modularización recomendada para React:

```text
Marketplace-universitario-Taller-1/
├── .gitignore                      # Exclusión de node_modules, dist y temporales
├── index.html                      # Plantilla HTML5 con Bootstrap 5 y montaje en #root
├── package.json                    # Dependencias y scripts de ejecución
├── vite.config.js                  # Configuración de Vite con @vitejs/plugin-react
├── README.md                       # Documentación técnica completa (pauta oficial)
├── public/
│   └── assets/img/                 # Recursos gráficos institucionales (logo UNAB, iconos)
└── src/
    ├── main.jsx                    # Punto de entrada de React (createRoot y StrictMode)
    ├── App.jsx                     # Componente contenedor raíz (estado global, filtros y handlers)
    ├── data/
    │   └── products.json           # Fuente de datos inicial en formato JSON
    ├── services/
    │   └── openLibraryApi.js       # Servicio asíncrono desacoplado para Open Library API
    ├── styles/
    │   └── styles.css              # Reglas de estilo personalizadas con identidad UNAB
    └── components/
        ├── Navbar.jsx              # Barra superior con logo, badges condicionales y accesos
        ├── Sidebar.jsx             # Filtros reactivos (texto, rango de precio, categorías y reset)
        ├── ProductCard.jsx         # Tarjeta de producto responsiva con formato CLP y botones
        ├── ProductDetailModal.jsx  # Modal centrado con información completa y vendedor
        ├── ProductFormModal.jsx    # Formulario controlado de publicación con validaciones
        ├── CartOffcanvas.jsx       # Panel lateral de carrito con cálculo total por .reduce()
        ├── FavoritesOffcanvas.jsx  # Panel lateral de favoritos con traspaso a carrito
        └── BookSearchModal.jsx     # Modal de consulta bibliográfica con estados loading/error
```

---

## 🌐 5. Integración de API Pública (Open Library)

En estricto cumplimiento con la **Sección 7** de la pauta:

- **Nombre de la API:** Open Library Search API.
- **URL oficial de documentación:** [https://openlibrary.org/developers/api](https://openlibrary.org/developers/api)
- **Endpoint utilizado:** `https://openlibrary.org/search.json?q={query}&limit=6`
- **Método HTTP:** `GET`
- **Parámetros enviados:** `q` (término de búsqueda codificado con `encodeURIComponent`) y `limit=6` (para restringir el volumen de respuesta y optimizar el rendimiento).
- **Datos de la respuesta utilizados:**
  * `doc.key`: Identificador único de la obra en Open Library.
  * `doc.title`: Título original del libro.
  * `doc.author_name`: Array de autores (normalizado como string).
  * `doc.first_publish_year`: Año de la primera edición.
  * `doc.cover_i`: Identificador numérico de la carátula para construir la URL de la portada (`https://covers.openlibrary.org/b/id/{cover_i}-M.jpg`).
- **Dónde se incorporan los datos en la interfaz:**
  1. En el modal [`BookSearchModal.jsx`](file:///c:/Users/amongusito/Desktop/marktTRY/Marketplace-universitario-Taller-1/src/components/BookSearchModal.jsx), mostrando tarjetas con portadas y metadatos.
  2. Al presionar **"Usar Datos para Publicar"**, se transmiten directamente al formulario [`ProductFormModal.jsx`](file:///c:/Users/amongusito/Desktop/marktTRY/Marketplace-universitario-Taller-1/src/components/ProductFormModal.jsx), autocompletando título, descripción académica, portada y categoría.
- **Manejo de Errores y Carga:**
  * **Loading:** Muestra un spinner centrado mientras la promesa está pendiente.
  * **Error:** Bloque de alerta Bootstrap si la red o el servicio falla (`try/catch` con `throw new Error()`).
  * **Empty State:** Mensaje amigable si la búsqueda no arroja coincidencias.
- **Restricciones y Autenticación:** Es una API libre y abierta que no requiere API Keys ni autenticación, orientada a consultas de bajo volumen.
- **Justificación funcional de valor:**  
  Evita que los estudiantes deban escribir manualmente autores, títulos extensos o buscar fotos de portada en Google. Con un solo clic, se autocompleta la ficha bibliográfica estandarizada para ramos de ingeniería, medicina, derecho, ciencias, etc.

---

## ⚡ 6. Funcionalidades Principales Implementadas

1. **Catálogo y Renderizado Dinámico:**
   - Visualización responsiva de productos mediante Grid de Bootstrap.
   - Formato de moneda local chilena con `precio.toLocaleString('es-CL')`.
   - Badges temáticos por categoría.

2. **Filtros Múltiples Reactivos (con `useMemo`):**
   - Búsqueda por palabra clave en tiempo real (coincidencia en nombre, descripción o vendedor).
   - Rango de precios dinámico con inputs de precio mínimo y máximo.
   - Filtro por categorías dinámicas con botón para limpiar todos los filtros a su estado inicial.

3. **Formulario Controlado y Validaciones:**
   - Estado local de formulario mediante `useState`.
   - Validación de campos requeridos (nombre, precio > 0, categoría, descripción >= 10 caracteres, vendedor).
   - Mensajes de error visuales y bloqueo de envío si hay datos inválidos.

4. **Carrito de Compras con Cálculo `.reduce()`:**
   - Panel lateral deslizable (Offcanvas).
   - Total acumulado estrictamente calculado con el método funcional `.reduce()`:
     ```javascript
     const totalPagar = cart.reduce((acum, item) => acum + Number(item.precio || 0), 0);
     ```
   - Control de duplicados, eliminación individual y vaciado de carrito.

5. **Lista de Favoritos:**
   - Guardado con botón toggle de corazón en cada producto.
   - Panel lateral de favoritos con opción de enviarlos directamente al carrito de compras.

6. **Persistencia Local:**
   - Sincronización automática de `cart` y `favorites` en el `localStorage` del navegador mediante `useEffect`.

---

## 🤖 7. Uso Responsable de Inteligencia Artificial (Reporte Sección 13.1)

| Elemento | Información Reportada |
| :--- | :--- |
| **Herramienta utilizada** | Antigravity AI Assistant (Google DeepMind) y ChatGPT. |
| **Propósito** | Apoyo en la arquitectura modular de componentes React 18, estructuración de estilos CSS para identidad UNAB, depuración del flujo de autocompletado asíncrono con Open Library y configuración de Vite. |
| **Prompt o consulta representativa** | *"Actúa como desarrollador Frontend experto en React 18, JSX, Vite y Bootstrap 5. Genera la estructura de carpetas y componentes para Marketplace UNAB según los requerimientos del Taller Evaluado 2, integrando la API de Open Library para autocompletar el formulario de publicación."* |
| **Resultado obtenido** | Esqueleto inicial de componentes, servicio `openLibraryApi.js` con fetch, estilos base y estructura de estados con hooks de React. |
| **Modificación humana y adaptación** | - Se corrigieron y ajustaron las rutas relativas de imágenes públicas (`public/assets/img/`).<br>- Se preservó y restauró el logo institucional original del Taller 1 evitando duplicación de texto en la barra superior.<br>- Se ajustaron las validaciones de campos requeridos y el formateo numérico a pesos chilenos (`es-CL`).<br>- Se resolvió la política de ejecución de scripts de Windows PowerShell (`Set-ExecutionPolicy`). |
| **Aprendizaje adquirido** | Comprensión profunda del ciclo de vida en React 18, la ventaja del filtrado derivado con `useMemo` frente al almacenamiento en estados redundantes, la gestión limpia de promesas asíncronas con estados triples (`loading`, `error`, `data`), y el desacoplamiento de componentes de presentación frente a componentes contenedores. |

---

## ⚠️ 8. Limitaciones Conocidas

- **Ausencia de Backend real:** Toda la gestión de publicaciones, carrito y favoritos opera en la memoria de la aplicación frontend y en el `localStorage` del navegador cliente. Si se borra la caché o se abre en modo incógnito, se restauran los productos base iniciales de `products.json`.
- **Transacciones simuladas:** La compra y el contacto con los vendedores son simulados mediante alertas amigables de interfaz, ya que no existe pasarela de pagos (Webpay/Transbank) ni servicio de mensajería backend.
- **Límites de Open Library:** Al ser un servicio público y gratuito, en ocasiones la descarga de portadas de libros muy antiguos o poco conocidos puede tardar algunos segundos o requerir un fallback visual por defecto.
