# Vogel Releases

Repositorio público de distribución de instaladores y metadatos de actualización para aplicaciones de Vogel Consultoría.

> Este repositorio **no contiene código fuente privado**. Solo publica artefactos de distribución, manifiestos de versión y checksums.

## Estructura

Cada aplicación usa un identificador estable (`app_id`) y publica un manifiesto en:

```text
apps/<app_id>/latest.json
```

Ejemplo:

```text
apps/femag/latest.json
apps/fgpy/latest.json
apps/forestal/latest.json
```

Los instaladores binarios se publican como assets de GitHub Releases. El manifiesto `latest.json` apunta al asset correspondiente.

## Formato de manifiesto

```json
{
  "schema_version": 1,
  "app_id": "femag",
  "version": "1.0.0",
  "published_at": "2026-08-27T00:00:00Z",
  "mandatory": false,
  "download_url": "https://github.com/oscarvogel/vogel-releases/releases/download/femag-v1.0.0/FEMAG-Setup-1.0.0.exe",
  "sha256": "",
  "notes": "Primera versión publicada mediante Vogel Releases."
}
```

## Convención de tags

Para evitar colisiones entre productos:

```text
femag-v1.2.3
fgpy-v2.0.1
forestal-v3.4.0
```

## Flujo previsto

1. El repositorio privado de cada aplicación ejecuta sus tests y genera el instalador.
2. Calcula SHA256 del artefacto.
3. Publica una GitHub Release en este repositorio.
4. Actualiza `apps/<app_id>/latest.json`.
5. La aplicación instalada consulta su manifiesto público y avisa al usuario si existe una versión superior.

## Seguridad

- Nunca publicar tokens, secretos, `.env`, credenciales ni código fuente privado.
- Las aplicaciones cliente no deben contener tokens de GitHub.
- Antes de instalar, el cliente debe validar el SHA256 publicado.
- Una falla al consultar actualizaciones nunca debe impedir iniciar o utilizar la aplicación.
