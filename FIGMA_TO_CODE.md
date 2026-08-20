# Figma a código

## Configuración

Este workspace registra el MCP remoto oficial de Figma en `.vscode/mcp.json`:

```json
{
  "servers": {
    "figma": {
      "type": "http",
      "url": "https://mcp.figma.com/mcp"
    }
  }
}
```

Al usarlo por primera vez, VS Code/Figma puede solicitar autorización OAuth. Completa ese flujo en el editor; no guardes tokens en el repositorio ni en variables de configuración compartidas.

## Uso

En GitHub Copilot, proporciona ambos datos en la misma petición:

```text
Convierte https://www.figma.com/design/FILE_KEY/Checkout?node-id=123-456
en web con React y CSS. Respeta los componentes y genera los archivos necesarios.
```

También puedes pedir plataformas nativas:

```text
Convierte https://www.figma.com/design/FILE_KEY/App?node-id=10-20 en android.
Genera Kotlin con Jetpack Compose.
```

```text
Convierte https://www.figma.com/design/FILE_KEY/App?node-id=10-20 en ios.
Genera SwiftUI con soporte para Dynamic Type.
```

Si no indicas la plataforma, la skill debe preguntarte antes de generar código. Las opciones válidas son `web`, `android` e `ios`.

## Tools de Figma

La skill usa principalmente `get_design_context` con el `fileKey` y `nodeId` obtenidos de la URL. Puede usar `get_metadata` para localizar nodos en archivos grandes y `get_screenshot` para comprobar detalles visuales.

La salida debe incluir la plataforma, el stack, los archivos afectados, los supuestos y los pasos de ejecución. La validación pixel-perfect requiere comparar la implementación ejecutándose con una captura del diseño.

## Comando de extensión

La configuración declarativa de [copilot-extensions.json](copilot-extensions.json) expone `figma-to-code`, con `figmaToCode.defaultTarget` configurable como `web`, `android` o `ios`.