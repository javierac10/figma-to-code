---
name: figma-to-code
description: "Convierte URLs de diseños de Figma en código para web, Android o iOS usando el MCP oficial de Figma. Usa esta skill cuando el usuario proporcione un enlace de Figma y pida implementar o inspeccionar una pantalla."
---

# Figma to Code

Usa el servidor MCP remoto `figma` registrado en `.vscode/mcp.json`:

`https://mcp.figma.com/mcp`

## Entrada requerida

Acepta una URL de Figma y una plataforma objetivo:

- `web`: HTML semántico, CSS y JavaScript o el framework web que indique el usuario.
- `android`: Kotlin y Jetpack Compose por defecto.
- `ios`: Swift y SwiftUI por defecto.

Si falta la plataforma, pregunta si quiere `web`, `android` o `ios` antes de generar código. Si falta la URL, solicita un enlace de archivo, frame o nodo de Figma.

## Flujo

1. Valida que la URL pertenezca a `figma.com` y conserva el enlace original.
2. Extrae `fileKey` y `nodeId` del enlace cuando estén disponibles. Normaliza `-` como separador de `nodeId` a `:`.
3. Llama al tool `get_design_context` del MCP de Figma con el `fileKey`, `nodeId` y el lenguaje/framework correspondiente.
4. Usa `get_screenshot` cuando haga falta verificar espaciado, estados, iconos o detalles visuales que no estén claros en el contexto.
5. Usa `get_metadata` si el archivo es grande o si necesitas localizar frames y componentes antes de pedir el contexto completo.
6. Genera código mantenible respetando la jerarquía visual, tipografía, colores, espaciado, estados y comportamiento responsive del diseño.
7. Reutiliza componentes y assets proporcionados por Figma. No sustituyas imágenes o iconos por placeholders sin indicarlo.
8. Ejecuta las comprobaciones disponibles del proyecto y reporta cualquier dependencia o decisión que no pueda inferirse del diseño.

## Contratos por plataforma

### Web

Entrega HTML semántico, CSS responsive y JavaScript solo cuando haya interacción. Si el proyecto usa React, Vue u otro framework, sigue sus convenciones existentes. Comprueba al menos escritorio y móvil cuando exista una vista ejecutable.. El contenido debe guardarse en un directorio llamado web_(codigo_alearorio) para evitar conflictos con otros assets.

### Android

Entrega Kotlin con Jetpack Compose por defecto. Incluye estados de UI, recursos accesibles, dimensiones adaptables y una estructura que pueda integrarse en una pantalla existente. Usa XML solo si el proyecto ya está basado en Views. El contenido debe guardarse en un directorio llamado android_(codigo_alearorio) para evitar conflictos con otros assets.

### iOS

Entrega Swift con SwiftUI por defecto. Incluye previews cuando el proyecto las use, estados de UI, Dynamic Type, VoiceOver y layouts adaptables. Usa UIKit solo si el proyecto ya lo requiere. El contenido debe guardarse en un directorio llamado ios_(codigo_alearorio) para evitar conflictos con otros assets.

## Respuesta

Presenta siempre:

- URL de Figma utilizada.
- Plataforma y stack elegido.
- Archivos creados o modificados.
- Supuestos y elementos que requieren revisión visual.
- Cómo ejecutar o integrar el resultado.

No afirmes que el resultado es pixel-perfect sin validarlo contra una captura o el entorno de ejecución. No expongas tokens ni credenciales; la autenticación del MCP debe gestionarse mediante el flujo de VS Code/Figma.