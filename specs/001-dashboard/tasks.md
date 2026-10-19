# Tareas 001 — Dashboard GymDash

> Derivadas de [plan.md](plan.md). Criterios de aceptación en [spec.md](spec.md).
> Archivo a modificar: `index.html`.

## Fase 0 — Base del documento

- [x] **T-001** Cambiar `<html lang="en">` por `lang="es"`. *(RNF-06, CA-03)*
- [x] **T-002** Cambiar `<title>` a "GymDash · Panel del gimnasio". *(CA-03)*
- [x] **T-003** Añadir al `<body>` las clases `bg-gray-50 text-gray-900 antialiased min-h-screen`.
- [x] **T-004** Crear `<header>` sticky (`sticky top-0 z-10 bg-white border-b border-gray-200`) con `h1` del nombre del gimnasio.
- [x] **T-005** Añadir `<nav>` dentro del header con enlaces a `#kpis`, `#drivers`, `#operaciones`. *(RF-01, CA-01)*
- [x] **T-006** Hacer el header responsive: nav bajo el título en móvil, en línea desde `sm:` (`sm:flex-row sm:items-center sm:justify-between`).
- [x] **T-007** Crear `<main>` con `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-6 space-y-10`.
- [x] **T-008** Crear las tres `<section>` vacías (`#kpis`, `#drivers`, `#operaciones`) con su `h2` y `scroll-mt-24`. *(CA-02)*
- [x] **T-009** Crear `<footer>` con el texto "Datos de ejemplo · mes actual".
- [x] **T-010** Verificar con `python3 server.py` que la página carga y los enlaces del nav saltan a cada sección.

## Fase 1 — Bloque 1: KPIs

- [x] **T-011** Aplicar a `section#kpis` la rejilla `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4`.
- [x] **T-012** Maquetar la tarjeta patrón (`article` + etiqueta + valor + detalle) con las clases definidas en el plan. *(RNF-04)*
- [x] **T-013** Tarjeta "Socios activos": **245** · "Básica 85 · Estándar 120 · Premium 40".
- [x] **T-014** Tarjeta "Ingresos mensuales": **12.745 €** · "11.505 € membresías + 1.240 € otros".
- [x] **T-015** Tarjeta "Ocupación media de clases": **84,6 %** · "55 de 65 plazas".
- [x] **T-016** Tarjeta "Crecimiento neto": **+13** · "18 altas · 5 bajas".
- [x] **T-017** Comprobar que las 4 tarjetas usan exactamente las mismas clases. *(CA-05)*
- [x] **T-018** Verificar la rejilla a 375 px (1 col), 640 px (2×2) y 1024 px (4 col). *(CA-04, CA-14)*

## Fase 2 — Bloque 2: Drivers

- [x] **T-019** Aplicar a `section#drivers` la rejilla `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4`.
- [x] **T-020** Crear las 3 tarjetas contenedoras con el mismo estilo que los KPIs y un `h3` cada una.

### Desglose de ingresos

- [x] **T-021** Crear `<ul>` con la estructura de fila: etiqueta + importe/% (`flex justify-between text-sm`) y barra (pista `h-2 rounded-full bg-gray-200`).
- [x] **T-022** Fila Estándar: 5.880 € · 46,1 % · `w-[46.1%]`.
- [x] **T-023** Fila Premium: 3.160 € · 24,8 % · `w-[24.8%]`.
- [x] **T-024** Fila Básica: 2.465 € · 19,3 % · `w-[19.3%]`.
- [x] **T-025** Fila Otros ingresos: 1.240 € · 9,7 % · `w-[9.7%]`.
- [x] **T-026** Añadir `aria-hidden="true"` a las barras. *(RNF-08, CA-06)*

### Retención de socios

- [x] **T-027** Mostrar cifras en `grid grid-cols-2`: Altas **18** (verde) y Bajas **5** (rojo).
- [x] **T-028** Añadir barra apilada `flex h-3 overflow-hidden rounded-full` con `w-[78.3%]` verde y `w-[21.7%]` rojo.
- [x] **T-029** Añadir pie "Crecimiento neto +13". *(CA-07)*

### Top 3 clases por ocupación

- [x] **T-030** Crear `<ol>` reutilizando la estructura de barra del desglose de ingresos.
- [x] **T-031** Filas: HIIT 100 % (`w-full`), Pilates 100 % (`w-full`), Yoga Matinal 93 % (`w-[93%]`). *(CA-08)*
- [x] **T-032** Añadir `md:col-span-2 lg:col-span-1` a la 3.ª tarjeta.
- [x] **T-033** Verificar la rejilla a 375 px, 768 px y 1024 px. *(CA-14)*

## Fase 3 — Bloque 3: Detalles operacionales

- [x] **T-034** Aplicar a `section#operaciones` la rejilla `grid grid-cols-1 lg:grid-cols-2 gap-4`.
- [x] **T-035** Crear 2 tarjetas con `h3` y wrapper `overflow-x-auto` para cada tabla.

### Tabla de horario de clases

- [x] **T-036** Crear `<table>` con `<thead>` (`th scope="col"`): Clase, Aforo, Inscritos, Ocupación, Días. *(CA-11)*
- [x] **T-037** Añadir las 4 filas en `<tbody>` (`divide-y divide-gray-100`) con el nombre como `th scope="row"`. *(CA-09)*
- [x] **T-038** Ocultar Aforo, Inscritos y Días en móvil con `hidden sm:table-cell` (cabecera y celdas).
- [x] **T-039** Añadir bajo cada nombre la línea de resumen móvil `sm:hidden text-xs text-gray-500` (p. ej. "14/15 · Lun / Mié / Vie").
- [x] **T-040** Mostrar la ocupación como badge de color: 100 % "Completo", 93 % ámbar, 50 % verde.

### Tabla de tipos de membresía

- [x] **T-041** Crear `<table>` con `<thead>` (`th scope="col"`): Tipo, Precio, Socios activos, Ingresos. *(CA-11)*
- [x] **T-042** Añadir las 3 filas (Básica 29 € · 85 · 2.465 €; Estándar 49 € · 120 · 5.880 €; Premium 79 € · 40 · 3.160 €).
- [x] **T-043** Alinear cifras con `text-right tabular-nums`.
- [x] **T-044** Añadir `<tfoot>` en `font-semibold`: Total · 245 · 11.505 €. *(CA-10)*
- [x] **T-045** Verificar ambas tablas a 320 px sin scroll horizontal de página. *(CA-13)*

## Fase 4 — Pasada responsive y QA

- [x] **T-046** Revisar el layout a 320, 375, 640, 768, 1024 y 1280 px.
- [ ] **T-047** Ejecutar en DevTools a 320 px: `document.documentElement.scrollWidth <= document.documentElement.clientWidth`. *(CA-13)*
- [ ] **T-048** Buscar `style` en `index.html`: sin `<style>` ni atributos `style=""`. *(CA-12)*
- [ ] **T-049** Revisar formato numérico es-ES (miles `.`, decimal `,`, `€` tras la cifra). *(RNF-07)*
- [ ] **T-050** Contrastar todos los valores con la spec (CA-04 a CA-10).
- [ ] **T-051** Validar el HTML en el validador W3C y la jerarquía h1 → h2 → h3. *(RNF-01, RNF-08)*
- [ ] **T-052** Comprobar que cada enlace del nav no deja la sección oculta bajo el header. *(CA-01)*
- [ ] **T-053** Verificación final con `python3 server.py` en el puerto 3000. *(CA-15)*
