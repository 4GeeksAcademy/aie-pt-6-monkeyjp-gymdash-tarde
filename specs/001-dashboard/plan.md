# Plan 001 — Implementación del Dashboard GymDash

> Basado en [spec.md](spec.md). Las tareas se definen en un archivo aparte.

## Enfoque

Implementar todo el dashboard en un único `index.html` (Tailwind v4 vía CDN, sin CSS ni JS), construyendo de arriba a abajo: base + cabecera → Bloque 1 KPIs → Bloque 2 Drivers → Bloque 3 Detalles operacionales → pasada responsive/QA. Cada fase es verificable de forma independiente con `python3 server.py` (puerto 3000).

## Archivos

| Archivo | Acción |
|---|---|
| `index.html` | Único archivo a modificar |
| `server.py` | Reutilizar sin cambios para previsualizar |
| `specs/001-dashboard/spec.md` | Fuente de valores y criterios de aceptación |

## Fases

### Fase 0 — Base del documento *(bloquea al resto)*

1. `<head>`: `lang="es"`, `<title>GymDash · Panel del gimnasio</title>`, conservar el script de Tailwind v4.
2. `<body>`: `bg-gray-50 text-gray-900 antialiased min-h-screen`.
3. `<header>` `sticky top-0 z-10 bg-white border-b border-gray-200`:
   - `h1` con el nombre del gimnasio.
   - `<nav>` con enlaces a `#kpis`, `#drivers`, `#operaciones` (`flex flex-wrap gap-x-4 text-sm`).
   - Móvil: nav bajo el título; `sm:flex-row sm:items-center sm:justify-between`.
4. `<main>`: `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-6 space-y-10`.
5. Tres `<section>` con `id`, `h2` y `scroll-mt-24` (compensa el header sticky).
6. `<footer>` simple ("Datos de ejemplo · mes actual").

### Fase 1 — Bloque 1: KPIs *(depende de Fase 0; paralelizable con Fases 2 y 3)*

1. `section#kpis`: `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4`.
2. Patrón de tarjeta único, idéntico en las 4 tarjetas:
   - Contenedor: `<article>` `rounded-xl bg-white p-6 shadow-sm ring-1 ring-gray-200`.
   - Etiqueta: `text-sm font-medium text-gray-500`.
   - Valor: `mt-2 text-3xl font-bold text-gray-900`.
   - Detalle: `mt-1 text-sm text-gray-500`.
3. Tarjetas:

| KPI | Valor | Detalle |
|---|---|---|
| Socios activos | 245 | Básica 85 · Estándar 120 · Premium 40 |
| Ingresos mensuales | 12.745 € | 11.505 € membresías + 1.240 € otros |
| Ocupación media de clases | 84,6 % | 55 de 65 plazas |
| Crecimiento neto | +13 | 18 altas · 5 bajas |

4. Con 4 tarjetas la rejilla queda equilibrada (1 → 2×2 → 4) sin necesidad de `col-span`.

### Fase 2 — Bloque 2: Drivers *(depende de Fase 0; paralelizable con Fases 1 y 3)*

1. `section#drivers`: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4`; mismo contenedor de tarjeta que los KPIs y un `h3` por tarjeta.
2. **Desglose de ingresos** (`<ul>`, orden descendente):
   - Fila: etiqueta + importe (`flex justify-between text-sm`) y barra debajo.
   - Pista `h-2 rounded-full bg-gray-200`; relleno `h-full rounded-full` con:
     - Estándar 5.880 € → `w-[46.1%]`
     - Premium 3.160 € → `w-[24.8%]`
     - Básica 2.465 € → `w-[19.3%]`
     - Otros ingresos 1.240 € → `w-[9.7%]`
   - Porcentaje visible en texto; la barra lleva `aria-hidden="true"`.
3. **Retención de socios**:
   - Dos cifras grandes en `grid grid-cols-2`: Altas **18** (verde), Bajas **5** (rojo).
   - Barra apilada `flex h-3 overflow-hidden rounded-full`: `w-[78.3%]` verde + `w-[21.7%]` rojo (18/23 y 5/23).
   - Pie: "Crecimiento neto +13".
4. **Top 3 clases por ocupación** (`<ol>`), misma estructura de barra que ingresos:
   - HIIT 100 % → `w-full`
   - Pilates 100 % → `w-full`
   - Yoga Matinal 93 % → `w-[93%]`
5. En `md` (2 columnas) la 3.ª tarjeta ocupa el ancho completo: `md:col-span-2 lg:col-span-1`.

### Fase 3 — Bloque 3: Detalles operacionales *(depende de Fase 0; paralelizable con Fases 1 y 2)*

1. `section#operaciones`: `grid grid-cols-1 lg:grid-cols-2 gap-4`. Cada tabla en un `<article>` tarjeta con `h3` y wrapper `overflow-x-auto` (red de seguridad: el scroll queda dentro de la tarjeta, nunca en la página).
2. **Tabla de horario de clases**:
   - `table w-full text-left text-sm`; `<thead>` con `th scope="col"`; nombre de clase como `th scope="row"`; `<tbody>` con `divide-y divide-gray-100`.
   - Columnas: Clase, Aforo, Inscritos, Ocupación, Días.
   - Móvil: Aforo, Inscritos y Días con `hidden sm:table-cell`; bajo el nombre, línea `sm:hidden text-xs text-gray-500` con el resumen (p. ej. "14/15 · Lun / Mié / Vie").
   - Ocupación como badge: 100 % ámbar/rojo "Completo", 93 % ámbar, 50 % verde.

| Clase | Aforo | Inscritos | Ocupación | Días |
|---|---|---|---|---|
| Yoga Matinal | 15 | 14 | 93 % | Lun / Mié / Vie |
| HIIT | 20 | 20 | 100 % | Mar / Jue |
| Spinning | 18 | 9 | 50 % | Lun / Mié |
| Pilates | 12 | 12 | 100 % | Mar / Vie |

3. **Tabla de tipos de membresía**:
   - Columnas: Tipo, Precio, Socios activos, Ingresos; cifras `text-right tabular-nums`.
   - `<tfoot>` en `font-semibold`: Total — 245 socios — 11.505 €.
   - 4 columnas cortas caben en 320 px con `px-3`.

### Fase 4 — Pasada responsive y QA *(depende de Fases 1–3)*

1. Revisar a 320, 375, 640, 768, 1024 y 1280 px.
2. Confirmar que las 4 tarjetas KPI tienen clases idénticas.
3. Revisar formato numérico es-ES en todo el documento.

## Verificación

1. `python3 server.py` y abrir el puerto 3000 (CA-15).
2. DevTools en modo responsive a 320 px: `document.documentElement.scrollWidth <= document.documentElement.clientWidth` (CA-13).
3. Buscar `style` en `index.html`: sin etiquetas `<style>` ni atributos `style=""` (CA-12).
4. Checklist de valores frente a la spec: 245, 12.745 €, 84,6 %, +13, 2.465 / 5.880 / 3.160 / 1.240 €, 18 / 5, top 3 (CA-04 a CA-10).
5. Validador W3C sin errores; jerarquía de encabezados h1 → h2 → h3 (RNF-08).
6. Clic en cada enlace del nav: la sección no queda oculta bajo el header (CA-01).

## Decisiones

- Un único `index.html`, sin JavaScript; valores precalculados.
- 4 KPIs (se añade "Crecimiento neto +13") para equilibrar la rejilla.
- Barras con `div` y anchos arbitrarios `w-[x%]` (soportados por el CDN de Tailwind v4).
- Tablas en móvil: columnas secundarias ocultas + resumen en línea, con `overflow-x-auto` como red de seguridad.
- Paleta: neutros `gray` + acento `emerald`; verde/rojo/ámbar solo para estados.
- Fuera de alcance: librerías de gráficos, JavaScript, modo oscuro.
