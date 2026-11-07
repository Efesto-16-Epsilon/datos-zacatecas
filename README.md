# Hospedaje permanente de datos.json — Mapa de Zacatecas

Fecha de preparación: 2026-11-06. Estado: listo para subir, falta la decisión de Luis (su cuenta de GitHub).

## Por qué

La APK v10 descarga `datos.json` desde un enlace de Muse que **caduca el 2026-11-08 ~07:23 UTC**.
Mientras esté vivo, actualizar datos no exige reconstruir el APK; cuando caduque, la app
sigue con los datos embebidos y renovar exige re-subir y reconstruir el APK cambiando solo la URL.
Este paquete elimina esa caducidad: la URL nueva no expira.

## Qué hay en esta carpeta

- `datos.json` — copia byte por byte de `../data/datos.json` v11.0 (2026-11-06, 42 zonas, 11 fuentes).
  MD5 verificado: `133c062df955c799330e2809117c916c` (igual que el original).
- `index.html` — página mínima para personas, con enlace a `datos.json`.
- `config.json` — configuración de Supabase para la app v12: la URL del proyecto (ya viene puesta) y la clave pública anon. Antes de subirlo, sustituye `PEGA_AQUI_TU_PUBLISHABLE_KEY` por la publishable key de Settings → API. La clave pública está diseñada por Supabase para ir dentro de aplicaciones cliente; el acceso real lo controlan las reglas RLS y las cuentas que crea Luis.
- Este README.

## Opción A (recomendada): GitHub Pages

1. En GitHub, crear un repositorio **público** nuevo, por ejemplo `datos-zacatecas`.
2. Subir estos dos archivos (`datos.json`, `index.html`) a la rama principal.
3. Settings → Pages → Source: Deploy from a branch → rama principal, carpeta `/ (root)`.
4. La URL queda: `https://<usuario>.github.io/datos-zacatecas/datos.json`
5. Actualizar datos después = sustituir `datos.json` en el repo (un commit). La app lo ve sola, sin tocar el APK.

## Opción B (sin activar Pages): enlace raw de GitHub

Con el mismo repositorio público, la app puede leer directamente:
`https://raw.githubusercontent.com/<usuario>/datos-zacatecas/main/datos.json`

## Verificado el 2026-11-06 (curl, con Origin de ejemplo)

- `raw.githubusercontent.com` responde `Access-Control-Allow-Origin: *` — el `fetch()` de la app lo puede leer.
- `*.github.io` (GitHub Pages, probado en mozilla.github.io y w3c.github.io) también responde
  `Access-Control-Allow-Origin: *`.
- Ambas URLs son gratuitas y sin caducidad mientras el repositorio sea público.

## Qué cambia en el APK (v12)

La app ya no lee los datos de este enlace de GitHub como fuente principal: los lee de Supabase (tabla app_datos). Lo que sí lee de aquí es `config.json`, al abrir, para saber la dirección del proyecto y la clave pública. Si `config.json` no está o no carga, la app sigue con los datos embebidos y el formulario guardando en el móvil.

## Qué falta (decisión de Luis)

- Su usuario de GitHub (o que cree el repositorio él y me pase el nombre), o elegir un servidor propio.
- Aprobar que los archivos sean públicos: son solo agregados oficiales, sin datos personales; la clave pública de Supabase está diseñada para ir en aplicaciones cliente.
- Subir `config.json` con la publishable key ya puesta (la URL del proyecto ya viene en el archivo).
- Después de eso, construir la APK v12 leyendo `config.json` de la URL nueva.
