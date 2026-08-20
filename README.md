# Figma to Code Agent

Agente de IA para convertir diseños de Figma en implementaciones para web, Android o iOS usando el servidor MCP oficial de Figma. El flujo acepta una URL de diseño y una plataforma objetivo, inspecciona el contexto visual y genera código alineado con el stack del proyecto.

## 📋 Descripción General

La integración principal está en [FIGMA_TO_CODE.md](FIGMA_TO_CODE.md) y en la skill [figma-to-code](.github/skills/figma-to-code/SKILL.md). El servidor remoto se registra en [.vscode/mcp.json](.vscode/mcp.json) con el endpoint `https://mcp.figma.com/mcp`.
