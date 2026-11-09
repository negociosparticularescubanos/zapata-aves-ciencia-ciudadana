# Ciénaga de Zapata — Avistamiento de aves + ciencia ciudadana

App web completa (móvil y escritorio) para que guías locales y turistas registren
avistamientos de aves endémicas de Cuba en la Ciénaga de Zapata, con GPS en mapa,
conteo por especie y evidencia (fotos / video / notas de campo).

## Arquitectura

- `main.pjs` — solo `$meta` (título, descripción, tags) + listas mínimas.
  Toda la lógica vive en `index.html` para que funcione sin dependencias de
  `src/` y sin necesidad de service worker en vista previa sin guardar.
- `index.html` — SPA completa, sin build:
  - CSS propio (mobile-first, responsive).
  - Leaflet 1.9.4 por CDN para los mapas (OpenStreetMap).
  - JS propio: almacenamiento en `localStorage` (offline-first),
    compresión de fotos en `<canvas>`, GPS con `navigator.geolocation`,
    exportación CSV/JSON, panel científico y validación por guías.
  - Sin emojis en la UI (SVG inline).

## Datos

- 23 especies endémicas presentes en Zapata (de las 27 de Cuba) definidas en
  `window.ZAPATA_SPECIES` dentro de `index.html`.
- Clave de almacenamiento: `zapata_aves_v1` (array de registros).
  Clave de guía seleccionado: `zapata_guia_asignado`.
- 7 registros semilla realistas para demostrar mapa y estadísticas.
  Se generan solo si no hay datos guardados.

## Multimedia (fotos y cantos)

- Fotos: miniaturas de Wikimedia Commons vía API de Wikipedia, con enlace
  a la ficha de cada especie. Cargan con `loading="lazy"` y tienen
  alternativa sin conexión. Crédito en el pie de la app.
- Cantos integrados: 18 de las 23 especies traen reproductor con grabaciones
  reales de iNaturalist (con crédito al grabador y enlace a la observación).
  La ficha de cada especie incluye además un botón de búsqueda en línea
  (API de iNaturalist, sin clave) y el enlace a su página en xeno-canto.
  5 especies ultra-raras (Fermina, Gallinuela, Cabrerito, Negrito, Chillina)
  aún no tienen grabaciones verificables en fuentes abiertas.
- Cada especie trae `clave` (rasgo visual diagnóstico) y `confunde`
  (especie parecida y cómo separarla) para ayudar en el campo.

## Probar

1. Recargar la vista previa.
2. Ir a "Registrar", pulsar "Usar mi GPS" (pide permiso), añadir especies
   con cantidades, adjuntar foto, guardar.
3. Ir a "Mapa" y comprobar el punto nuevo, filtros y popup.
4. Ir a "Datos" y exportar CSV/JSON.

## Notas para futuras sesiones

- No mover a `src/` sin motivo: la app está deliberadamente autocontenida.
- Leaflet requiere internet para las teselas. Sin internet el formulario
  sigue funcionando con lat/lng manuales.
- Videos grandes (>8 MB) no se persisten en `localStorage`: se guarda solo
  el nombre/tamaño y se avisa. Fotos se comprimen a max 1280px JPEG 0.8.
