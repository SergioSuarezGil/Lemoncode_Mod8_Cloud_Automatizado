# Lemoncode Módulo 8 · Cloud · Despliegue Automatizado

Ejercicio del Máster Frontend de Lemoncode (Módulo 8 — Cloud).
Despliegue **automatizado** de una aplicación frontend en **GitHub Pages**
mediante **GitHub Actions**.

## Enunciado

> Automatizar el proceso de despliegue: cada vez que se haga un merge a la
> rama principal, debe dispararse un flujo de build y despliegue usando
> GitHub Actions.

## Aplicación desplegada

Este repositorio **no contiene el código fuente**. El workflow clona el
repositorio público [Portfolio](https://github.com/SergioSuarezGil/Portfolio)
y lo construye con Vite usando una variable `BASE_PATH` específica para
este repo, de forma que el mismo código sirva en varios entornos
(producción, ejercicio manual, ejercicio automatizado).

**URL del despliegue:** https://sergiosuarezgil.github.io/Lemoncode_Mod8_Cloud_Automatizado/

## Cómo funciona el workflow

El archivo [`.github/workflows/deploy.yml`](./.github/workflows/deploy.yml)
define dos jobs encadenados:

1. **build**
   - `actions/checkout@v4` con `repository: SergioSuarezGil/Portfolio` para
     clonar el código fuente desde el repo externo.
   - `actions/setup-node@v4` (Node 20, caché de npm).
   - `npm ci` para instalar dependencias de forma reproducible.
   - `npm run build` con `BASE_PATH=/Lemoncode_Mod8_Cloud_Automatizado/`
     para que Vite genere rutas relativas correctas.
   - `actions/upload-pages-artifact@v3` sube el directorio `dist/` como
     artifact de GitHub Pages.

2. **deploy**
   - `actions/deploy-pages@v4` publica el artifact en el entorno
     `github-pages` y expone la URL del despliegue.

## Disparadores

- `push` a la rama `main` → despliegue automático.
- `workflow_dispatch` → permite ejecutarlo manualmente desde la pestaña
  **Actions** de GitHub.
