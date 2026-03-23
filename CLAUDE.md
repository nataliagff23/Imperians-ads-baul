# Imperians Agency · Bóveda de Anuncios Ganadores

## Descripción General

Aplicación web SPA (Single Page Application) de gestión de anuncios ganadores para **Imperians Agency**. Funciona como repositorio centralizado para almacenar, organizar, analizar y exportar anuncios de alto rendimiento en plataformas Meta (Facebook/Instagram).

**Stack técnico:**
- React 18 (cargado desde CDN, sin build step)
- Babel Standalone para transpilación JSX en el navegador
- Airtable como base de datos backend (REST API)
- CSS inline con React style objects, sin framework externo
- Google Fonts: Poppins, Cinzel, Playfair Display, Space Mono

**Archivo único:** `index.html` — toda la lógica, estilos y componentes en un solo archivo.

---

## Autenticación

- Pantalla de login con contraseña única (`imperians2025`)
- Sesión almacenada en `sessionStorage` (key: `imp_auth`)
- Animación de shake en contraseña incorrecta
- Botón de logout en el header principal

---

## Vistas Principales

### 1. Dashboard
- Vista por defecto tras el login
- Muestra tarjetas resumen por equipo (cuando el filtro "Todos" está activo)
- Cada tarjeta muestra conteo de anuncios y badge de winners del equipo

### 2. Galería (Grid de Anuncios)
- Grid responsivo con mínimo 340px por tarjeta (auto-fill)
- Cada tarjeta muestra: thumbnail/media, cliente, campaña, copy preview, plataforma, equipo, status badge y fecha
- Hover con elevación, borde destacado y sombra
- Click abre modal de detalle o togglea selección en modo multi-select

### 3. Modal de Detalle de Anuncio
- Backdrop con blur, modal centrado
- Muestra: nombre del cliente, campaña, status badge, team badge, fecha
- Preview de media (imagen o video con controles)
- Sección de copy: headline, texto principal, botón CTA
- Información de audiencia/targeting
- Notas y aprendizajes (destacadas en dorado)
- Botones: Editar y Eliminar

### 4. Modal de Formulario (Crear/Editar)
- Grid con múltiples grupos de campos
- Secciones: info básica, configuración de campaña, copy, targeting, media, notas
- Campo cliente con autocompletado (datalist dinámico)
- Zona de carga de imagen/video (máx 4MB)
- Botones: Cancelar y Guardar

---

## Estructura de Datos por Anuncio

### Información de Campaña
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | string | ID único |
| `airtableId` | string | ID del registro en Airtable |
| `client` | string | Nombre del cliente/marca |
| `campaignName` | string | Nombre de la campaña |
| `platform` | enum | Plataforma Meta |
| `adFormat` | enum | Formato del creativo |
| `objective` | enum | Objetivo de campaña |
| `status` | enum | Estado del anuncio |
| `team` | enum | Equipo asignado |
| `date` | string | Fecha (YYYY-MM-DD) |
| `audience` | string | Descripción de audiencia objetivo |

### Copy del Anuncio
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `headline` | string | Titular principal |
| `primaryText` | string | Texto body/descripción |
| `cta` | enum | Texto del call-to-action |

### Métricas de Rendimiento (solo display, calculadas desde Airtable)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `spend` | number | Presupuesto gastado ($) |
| `impressions` | number | Impresiones totales |
| `clicks` | number | Clics totales |
| `ctr` | number | Click-through rate (%) |
| `cpc` | number | Costo por clic ($) |
| `conversions` | number | Conversiones totales |
| `cpa` | number | Costo por adquisición ($) |
| `roas` | number | Retorno sobre inversión publicitaria |

**Auto-cálculo de métricas:**
- CTR = (clicks / impressions) × 100
- CPC = spend / clicks
- CPA = spend / conversions

### Media
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `mediaData` | string | Base64 o URL de Airtable |
| `mediaType` | string | "image" o "video" |
| `mediaName` | string | Nombre original del archivo |
| `notes` | string | Aprendizajes clave, insights, notas del creativo |

---

## Opciones y Enumeraciones

### Plataformas (Meta Ads)
- Facebook
- Instagram
- Messenger
- Audience Network

### Formatos de Anuncio
- Imagen
- Video
- Carrusel
- Story
- Reel
- Collection

### Objetivos de Campaña
- Conversiones
- Ventas del Catálogo
- Generación de Leads
- Tráfico
- Reconocimiento
- Interacción

### Estados de Anuncio
| Status | Emoji | Color |
|--------|-------|-------|
| winner | 🏆 Winner | Verde (#34d399) |
| testing | 🧪 Testing | Amarillo (#fbbf24) |
| paused | ⏸ Paused | Gris (#94a3b8) |

### CTAs Disponibles
- Comprar Ahora
- Inscríbete Ahora
- Más Información
- Reservar Cita
- Descargar
- Contáctanos
- Registrarse
- Ver Más

---

## Equipos

Tres equipos con identidad visual propia:

| Equipo | Icono | Color | Descripción |
|--------|-------|-------|-------------|
| Equipo Trajano | 🏛️ | Ámbar (#f59e0b) | |
| Equipo Cleopatra | 👑 | Violeta (#a78bfa) | |
| Equipo Gladiador | ⚔️ | Rosa/Rojo (#f43f5e) | |

Cada equipo tiene: color de texto, fondo translúcido y borde diferenciado.

---

## Funcionalidades

### Filtros y Búsqueda
- Búsqueda de texto libre (cliente, campaña, headline, copy)
- Filtro por cliente (dropdown con clientes únicos)
- Filtro por status (Winner, Testing, Paused)
- Filtro por plataforma
- Filtro por equipo (con contador de anuncios)

### Ordenación
- Por fecha (más reciente)
- Por ROAS (mayor retorno)
- Por CTR (mayor tasa de clics)
- Por spend (mayor inversión)

### Modo Multi-Selección
- Activar/desactivar selección múltiple
- Checkbox visual en cada tarjeta
- Tarjetas seleccionadas con borde y fondo destacado
- Usar junto con exportación para exportar anuncios específicos

### Exportación
- Genera un reporte HTML estilizado en nueva ventana del navegador
- Incluye: logo de la agencia, nombre del cliente, fecha, conteo de anuncios
- Por cada anuncio: media, plataforma, formato, status, equipo, campaña, headline, copy, CTA, audiencia, notas
- Optimizado para imprimir como PDF (botón "🖨️ Guardar como PDF")
- Exporta los anuncios filtrados o los seleccionados (según modo de selección)

### Gestión de Media
- Upload de imagen o video (max 4MB)
- Validación de tipo (solo image/* y video/*)
- Previsualización antes de guardar
- Botones para reemplazar o eliminar media
- Formatos soportados: JPG, JPEG, PNG, GIF, WebP, MP4, MOV, AVI

---

## Integración con Airtable

**Base de datos:** Airtable (backend único de la aplicación)

| Configuración | Valor |
|--------------|-------|
| Base ID | `appT08Cl4u2lVE1Td` |
| Table ID | `tblbVSfTfYu1dmcTP` |
| Media Field ID | `fldi4PsEyOqwxlmPu` |
| Límite por request | 100 registros |

### Mapping de Campos (Airtable → App)

| Campo en Airtable | Campo en App |
|-------------------|-------------|
| ID | id |
| Cliente | client |
| Campaña | campaignName |
| Plataforma | platform |
| Formato | adFormat |
| Headline | headline |
| Copy | primaryText |
| CTA | cta |
| Objetivo | objective |
| Audiencia | audience |
| Estado | status |
| Fecha | date |
| Equipo | team |
| Notas | notes |
| Media | mediaData / mediaType / mediaName |

### Operaciones CRUD
- **GET** — Carga todos los registros al iniciar la app
- **POST** — Crea nuevo registro al guardar un anuncio nuevo
- **PATCH** — Actualiza registro existente al editar
- **DELETE** — Elimina registro al borrar un anuncio
- **Media Upload** — Usa la Content API de Airtable (`content.airtable.com`) para subir adjuntos en base64

---

## Arquitectura de Componentes React

```
App
├── Login                  (pantalla de autenticación)
└── AdVault                (contenedor principal con todo el estado)
    ├── Header             (navegación, filtros, búsqueda, botones de acción)
    ├── Dashboard View     (tarjetas de equipos)
    ├── Gallery View       (grid de tarjetas de anuncios)
    ├── Ad Detail Modal    (vista completa del anuncio seleccionado)
    ├── Ad Form Modal      (formulario crear/editar)
    └── StatusBadge        (componente reutilizable de estado)
```

### Estado principal en `AdVault`
| State | Tipo | Descripción |
|-------|------|-------------|
| `ads` | array | Lista de todos los anuncios |
| `view` | string | Vista activa (dashboard/gallery) |
| `selectedAd` | object\|null | Anuncio en modal de detalle |
| `showForm` | boolean | Visibilidad del formulario |
| `formData` | object | Estado del formulario |
| `filterClient/Status/Platform/Team` | string | Filtros activos |
| `searchQuery` | string | Texto de búsqueda |
| `sortBy` | string | Campo de ordenación |
| `isLoading` | boolean | Estado de carga de datos |
| `editingId` | string\|null | ID del anuncio en edición |
| `selectionMode` | boolean | Modo multi-selección |
| `selectedIds` | array | IDs seleccionados |

---

## Diseño Visual

### Paleta de Colores
| Uso | Color |
|-----|-------|
| Fondo principal | #0a0a0f (negro azulado profundo) |
| Acento primario | #6366f1 / #818cf8 (índigo) |
| Acento secundario | #8b5cf6 (violeta) |
| Texto principal | #e2e8f0 (gris claro) |
| Texto secundario | #94a3b8 / #64748b (gris pizarra) |
| Bordes | rgba(255,255,255,0.05–0.1) |
| Éxito (winner) | #34d399 |
| Advertencia (testing) | #fbbf24 |
| Error | #f43f5e |
| Notas/destacado | #fef3c7 (fondo dorado claro) |

### Tipografía
- **Poppins** — Texto UI, etiquetas, botones, body (principal)
- **Cinzel** — Logo y branding
- **Playfair Display** — Títulos de modales (elegancia serif)
- **Space Mono** — Métricas y números (monoespaciado)

### Efectos Visuales
- Glassmorphism en modales (backdrop blur)
- Gradientes radiales de profundidad en fondos
- Sombras sutiles en elevación
- Hover con transform y cambio de borde en tarjetas
- Animaciones de entrada (fade-in + translateY) en modales
- Animación shake en login incorrecto

---

## Datos de Ejemplo (DEFAULT_DATA)

La app incluye 3 anuncios de muestra pre-cargados (usados como fallback si Airtable está vacío):

1. **FitLife Pro** — Summer Body Challenge — Instagram Reel — Equipo Trajano — 🏆 Winner — ROAS: 8.4
2. **Casa Bella Muebles** — Rebajas Enero — Facebook Carrusel — Equipo Cleopatra — 🏆 Winner — ROAS: 5.2
3. **Dr. Smile Dental** — Blanqueamiento Promo — Instagram Story — Equipo Gladiador — 🏆 Winner — ROAS: 6.1

---

## Funciones Utilitarias

| Función | Descripción |
|---------|-------------|
| `formatNum(n)` | Abrevia números grandes (K, M) |
| `formatMoney(n)` | Formatea moneda con $ y 2 decimales |
| `recordToAd(record)` | Convierte registro Airtable → objeto ad |
| `adToFields(ad)` | Convierte objeto ad → campos Airtable |
| `uploadMediaToAirtable()` | Sube media en base64 a Airtable Content API |

---

## Limitaciones Actuales / No Implementado

- Métricas KPI no son editables desde el formulario (solo display desde Airtable)
- Sin duplicación de anuncios
- Sin sistema de tags o etiquetas adicionales
- Sin historial de revisiones
- Sin tracking de A/B testing
- Sin gráficas de analytics en el dashboard
- Sin sincronización automática (solo carga manual al inicio)
- Carga máxima de 100 registros por request (sin paginación)
- Sin colaboración o comentarios multi-usuario
