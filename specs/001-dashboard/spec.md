# Spec 001 — Dashboard del Gimnasio (GymDash)

## Resumen

Dashboard web estático para la gestión de un gimnasio, construido con **HTML semántico** y **Tailwind CSS v4** (vía CDN `@tailwindcss/browser@4`). Presenta la información en tres bloques verticales: KPIs, Drivers y Detalles Operacionales.

## Contexto del proyecto

| Elemento | Estado | Uso |
|---|---|---|
| `index.html` | Esqueleto HTML5 con Tailwind v4 por CDN | Base del dashboard (único archivo a modificar) |
| `server.py` | Servidor Flask en puerto 3000, sin caché | Se reutiliza sin cambios para previsualizar |
| `learn.json`, `README.*` | Metadatos de plantilla | Sin cambios |

- No se requieren dependencias adicionales, build ni archivos CSS/JS.
- Los datos son estáticos y los valores derivados se precalculan en el HTML.

## Datos fuente

### Tipos de membresía

| Tipo | Precio mensual | Socios activos |
|---|---|---|
| Básica | 29 € | 85 |
| Estándar | 49 € | 120 |
| Premium | 79 € | 40 |

### Clases de esta semana

| Clase | Aforo | Inscritos | Día |
|---|---|---|---|
| Yoga Matinal | 15 | 14 | Lun / Mié / Vie |
| HIIT | 20 | 20 | Mar / Jue |
| Spinning | 18 | 9 | Lun / Mié |
| Pilates | 12 | 12 | Mar / Vie |

### Resumen mensual

- Nuevos socios: **18**
- Bajas: **5**
- Crecimiento neto: **+13**
- Ingresos totales: **12.745 €**

## Valores derivados (decisiones tomadas)

### Ingresos

| Concepto | Cálculo | Importe | % del total |
|---|---|---|---|
| Básica | 85 × 29 € | 2.465 € | 19,3 % |
| Estándar | 120 × 49 € | 5.880 € | 46,1 % |
| Premium | 40 × 79 € | 3.160 € | 24,8 % |
| **Subtotal membresías** | | **11.505 €** | 90,3 % |
| Otros ingresos | 12.745 − 11.505 | 1.240 € | 9,7 % |
| **Total** | | **12.745 €** | 100 % |

> **Decisión:** la diferencia de 1.240 € se presenta como **"Otros ingresos"**.

### Ocupación de clases

| Clase | Inscritos / Aforo | % ocupación |
|---|---|---|
| Yoga Matinal | 14 / 15 | 93 % |
| HIIT | 20 / 20 | 100 % |
| Spinning | 9 / 18 | 50 % |
| Pilates | 12 / 12 | 100 % |
| **Total** | **55 / 65** | **84,6 %** |

> **Decisión:** la tasa media de ocupación es **84,6 %** (ponderada: total inscritos / total aforo).

- Top 3 por ocupación: **HIIT (100 %)**, **Pilates (100 %)**, **Yoga Matinal (93 %)**.

### Socios

- Total socios activos: 85 + 120 + 40 = **245**.
- Retención del mes: **18 altas** vs. **5 bajas** → neto **+13**.

## Requisitos funcionales

### RF-01 — Cabecera
- El dashboard incluye un `<header>` con el nombre del gimnasio y un `<nav>` con enlaces ancla a los tres bloques.

### RF-02 — Bloque 1: KPIs (superior)
Muestra 4 tarjetas KPI:
- Total de socios activos: **245**
- Ingresos mensuales: **12.745 €**
- Tasa media de ocupación de clases: **84,6 %**
- Crecimiento neto del mes: **+13**

### RF-03 — Bloque 2: Drivers (intermedio)
Muestra al menos 3 visualizaciones:
1. **Desglose de ingresos** por tipo de membresía + "Otros ingresos", con barras CSS proporcionales a su porcentaje.
2. **Retención de socios**: altas (18) vs. bajas (5) y crecimiento neto (+13).
3. **Top 3 clases** por ocupación con su porcentaje y barra de progreso.

### RF-04 — Bloque 3: Detalles operacionales (inferior)
1. **Tabla de horario de clases** con columnas: nombre, aforo, inscritos, % ocupación, días.
2. **Tabla de tipos de membresía** con columnas: tipo, precio, socios activos, ingresos; incluye fila de total.

## Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | HTML semántico: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<table>` (con `<thead>`, `<tbody>`, `<th scope>`), `<footer>`. |
| RNF-02 | Estilos exclusivamente con clases utility de Tailwind v4. Sin CSS personalizado (`<style>`, atributos `style`, archivos `.css`) ni otros frameworks. |
| RNF-03 | Diseño mobile-first: layout base de una columna, ampliado con breakpoints `sm:`, `md:`, `lg:`. |
| RNF-04 | Tarjetas KPI visualmente consistentes: misma estructura, mismo padding y misma jerarquía tipográfica (etiqueta / valor / detalle). |
| RNF-05 | Sin scroll horizontal de página en pantallas pequeñas (≥ 320 px). |
| RNF-06 | Documento en español: `lang="es"` y `<title>` descriptivo. |
| RNF-07 | Formato numérico español: separador de miles `.`, decimal `,`, símbolo `€` tras la cifra. |
| RNF-08 | Accesibilidad básica: contraste legible, encabezados jerárquicos (`h1` → `h2` → `h3`), barras con texto que exprese su valor. |

## Criterios de aceptación

### Estructura
- [x] **CA-01** — Existe un único `<header>` con `<nav>` cuyos enlaces llevan a `#kpis`, `#drivers` y `#operaciones`.
- [x] **CA-02** — `<main>` contiene exactamente tres `<section>` en este orden: KPIs, Drivers, Detalles operacionales, cada una con su `h2`.
- [x] **CA-03** — `<html lang="es">` y `<title>` distinto de "Document".

### KPIs
- [x] **CA-04** — Se muestran 4 tarjetas con los valores **245**, **12.745 €**, **84,6 %** y **+13**.
- [x] **CA-05** — Todas las tarjetas KPI comparten las mismas clases de contenedor, padding y tipografía para etiqueta y valor.

### Drivers
- [x] **CA-06** — El desglose de ingresos muestra Básica (2.465 €), Estándar (5.880 €), Premium (3.160 €) y Otros ingresos (1.240 €), con barras cuyo ancho corresponde a su % sobre 12.745 €.
- [x] **CA-07** — La retención muestra 18 altas, 5 bajas y +13 neto.
- [x] **CA-08** — El top 3 lista HIIT (100 %), Pilates (100 %) y Yoga Matinal (93 %), en orden descendente.

### Detalles operacionales
- [x] **CA-09** — La tabla de clases contiene las 4 clases con aforo, inscritos, % ocupación y días correctos.
- [x] **CA-10** — La tabla de membresías contiene los 3 tipos con precio, socios, ingresos y una fila total (245 socios / 11.505 €).
- [x] **CA-11** — Las tablas usan `<thead>`, `<tbody>` y `<th scope="col">`.

### Estilo y responsive
- [x] **CA-12** — No hay etiquetas `<style>`, atributos `style=""` ni hojas CSS enlazadas; solo se carga Tailwind v4 por CDN.
- [ ] **CA-13** — A 320–375 px todo se muestra en una columna y la página no tiene scroll horizontal (tablas incluidas).
- [x] **CA-14** — A partir de `sm:`/`md:`/`lg:` las tarjetas KPI y los drivers se distribuyen en varias columnas.
- [x] **CA-15** — El dashboard se visualiza correctamente al ejecutar `python3 server.py` y abrir el puerto 3000.

## Fuera de alcance

- Interactividad con JavaScript, carga dinámica de datos o backend.
- Librerías de gráficos externas.
- Autenticación, persistencia o edición de datos.
