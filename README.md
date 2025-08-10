# Ejemplo de servidor MCP remoto en Azure Container Apps

Este servidor MCP utiliza transporte **SSE** y se autentica con una **clave API**.

## 🚀 Ejecución local

### Requisitos previos
- Python **3.11** o superior  
- [uv](https://docs.astral.sh/uv/getting-started/installation/)

### Pasos
```bash
uv venv
uv sync

# linux/macOS
export API_KEYS=<AN_API_KEY>
# windows
set API_KEYS=<AN_API_KEY>

uv run fastapi dev main.py


Configuración de MCP en VS Code (mcp.json)
{
    "inputs": [
        {
            "type": "promptString",
            "id": "weather-api-key",
            "description": "Clave API del servicio de clima",
            "password": true
        }
    ],
    "servers": {
        "weather-sse": {
            "type": "sse",
            "url": "http://localhost:8000/sse",
            "headers": {
                "x-api-key": "${input:weather-api-key}"
            }
        }
    }
}

☁️ Desplegar en Azure Container Apps
az containerapp up -g <RESOURCE_GROUP_NAME> -n weather-mcp --environment mcp -l westus --env-vars API_KEYS=<AN_API_KEY> --source .
📌 Nota:
Si el despliegue es exitoso, la CLI de Azure devolverá la URL de la aplicación. Usa esta URL para conectarte al servidor desde Visual Studio Code.
🔄 Solución de problemas
Si el despliegue falla, intenta nuevamente actualizando la CLI y la extensión de Azure Container Apps:
az upgrade
az extension add -n containerapp --upgrade
