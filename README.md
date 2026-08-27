# Vogel Releases

Repositorio público de distribución de instaladores y metadatos de actualización para aplicaciones de Vogel Consultoría.

> Este repositorio **no contiene código fuente privado**. Solo publica artefactos de distribución, manifiestos de versión y checksums.

## Estrategia de almacenamiento

Se mantiene **un único GitHub Release permanente** con tag `latest`.

Ese release contiene solamente el instalador vigente de cada aplicación, por ejemplo:

```text
FEMAG_Desktop_Produccion_Setup.exe
FGPY_Mantenimiento_Preventivo.exe
Forestal_Setup.exe
```

Cuando una aplicación publica una versión nueva, el workflow elimina **solo el asset anterior de esa aplicación** y sube el nuevo. No se conservan instaladores históricos, evitando crecimiento innecesario del almacenamiento.

## Estructura de manifiestos

Cada aplicación usa un identificador estable (`app_id`) y publica su versión vigente en:

```text
apps/<app_id>/latest.json
```

Ejemplo:

```text
apps/femag/latest.json
apps/fgpy/latest.json
apps/forestal/latest.json
```

Los manifiestos apuntan al asset correspondiente del release `latest`.

## Formato de manifiesto

```json
{
  "schema_version": 1,
  "app_id": "femag",
  "version": "2026.08.27.16.30.00",
  "published_at": "2026-08-27T19:30:00Z",
  "mandatory": false,
  "download_url": "https://github.com/oscarvogel/vogel-releases/releases/download/latest/FEMAG_Desktop_Produccion_Setup.exe",
  "sha256": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "notes": "Mejoras y correcciones de FEMAG."
}
```

## Flujo previsto

1. El repositorio de cada aplicación ejecuta sus tests y genera el instalador.
2. Calcula SHA256 del artefacto.
3. Localiza el release permanente `latest` en este repositorio.
4. Elimina del release solamente el asset anterior de esa aplicación, si existe.
5. Sube el nuevo instalador manteniendo un nombre estable.
6. Actualiza `apps/<app_id>/latest.json` con versión, fecha, SHA256 y notas.
7. La aplicación instalada consulta su manifiesto público y avisa al usuario si existe una versión superior.

## Seguridad

- Nunca publicar tokens, secretos, `.env`, credenciales ni código fuente privado.
- Las aplicaciones cliente no deben contener tokens de GitHub.
- Antes de instalar, el cliente debe validar el SHA256 publicado.
- Una falla al consultar actualizaciones nunca debe impedir iniciar o utilizar la aplicación.
