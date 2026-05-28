# Canarias Tourism AI Assistant API

API REST desarrollada con FastAPI para integrar Claude AI como asistente de turismo de las Islas Canarias, implementando un sistema RAG (Retrieval-Augmented Generation) simple que utiliza datos estadísticos turísticos reales.

## 📋 Característicassss

- **Asistente IA especializado**: Claude Haiku 4.5 configurado específicamente para turismo en Canarias
- **Sistema RAG simple**: Recuperación de datos relevantes basada en la consulta del usuario
- **Autenticación simple**: API Key maestra para proteger los endpoints
- **CORS habilitado**: Preparado para integración con frontend
- **Documentación automática**: Swagger UI y ReDoc incluidos

## 🏗️ Arquitectura

```
tfm-ai-api/
├── main.py              # Aplicación FastAPI principal
├── src/
│   ├── config.py        # Configuración de la aplicación
│   ├── prompts.py       # Prompts del sistema (para documentación TFM)
│   └── rag.py           # Sistema RAG simple
├── data/
│   └── tourism_data.json # Datos turísticos (estadísticas reales)
├── requirements.txt     # Dependencias Python
├── .env.example         # Ejemplo de variables de entorno
└── README.md            # Este archivo
```

## 🚀 Instalación

### 1. Clonar e instalar dependencias

```bash
cd tfm-ai-api
pip install -r requirements.txt
```

### 2. Configurar variables de entorno

Crear archivo `.env` basado en `.env.example`:

```bash
cp .env.example .env
```

Editar `.env` con tus claves:

```env
ANTHROPIC_API_KEY=tu_clave_de_claude
MASTER_API_KEY=tu_clave_maestra_segura
```

### 3. Ejecutar en local

```bash
python main.py
```

La API estará disponible en: `http://localhost:8000`

## 📡 Endpoints

### `GET /`
Endpoint de bienvenida con información de la API.

**Respuesta:**
```json
{
  "message": "Canarias Tourism AI Assistant API",
  "version": "1.0.0",
  "status": "online"
}
```

### `GET /health`
Health check para verificar el estado del sistema.

**Respuesta:**
```json
{
  "status": "healthy",
  "rag_system": "initialized",
  "data_records": 1500
}
```

### `POST /chat`
Endpoint principal para interactuar con el asistente.

**Headers requeridos:**
```
X-API-Key: tu_master_api_key
Content-Type: application/json
```

**Request body:**
```json
{
  "message": "¿Cuántos turistas visitaron Tenerife en enero de 2025?"
}
```

**Response:**
```json
{
  "response": "Según los datos disponibles, en enero de 2025 Tenerife recibió aproximadamente..."
}
```

## 🧪 Ejemplos de uso

### cURL

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -H "X-API-Key: tu_master_api_key" \
  -d '{"message": "¿Cuál es la ocupación hotelera en Lanzarote?"}'
```

### Python

```python
import requests

url = "http://localhost:8000/chat"
headers = {
    "X-API-Key": "tu_master_api_key",
    "Content-Type": "application/json"
}
data = {
    "message": "¿Cuántos turistas visitaron Gran Canaria en verano?"
}

response = requests.post(url, json=data, headers=headers)
print(response.json())
```

### JavaScript/Fetch

```javascript
const response = await fetch('http://localhost:8000/chat', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-API-Key': 'tu_master_api_key'
  },
  body: JSON.stringify({
    message: '¿Cuál es la isla más visitada?'
  })
});

const data = await response.json();
console.log(data.response);
```

## 🔒 Seguridad

- **Autenticación**: Todas las peticiones requieren header `X-API-Key`
- **Validación**: Pydantic valida todos los inputs
- **Rate Limiting**: Preparado para implementar límites (configuración en `config.py`)
- **CORS**: Configurable según necesidades del frontend

## 📊 Sistema RAG

El sistema RAG implementado es simple pero efectivo:

1. **Recuperación**: Filtra datos por isla, fecha y palabras clave
2. **Agregación**: Calcula estadísticas resumidas cuando es relevante
3. **Contexto**: Proporciona datos estructurados a Claude
4. **Respuesta**: Claude genera respuestas naturales basadas en los datos

### Capacidades del RAG:

- Filtrado por isla específica
- Filtrado temporal (año/mes)
- Detección de métricas (ocupación, ingresos, turistas, etc.)
- Resúmenes estadísticos automáticos
- Priorización de datos recientes

## 🎯 Características del Asistente

El asistente está configurado para:

- ✅ Responder SOLO sobre turismo en Canarias
- ✅ Usar exclusivamente datos estadísticos proporcionados
- ✅ Citar cifras específicas y períodos
- ✅ Comparar islas cuando es apropiado
- ✅ Rechazar cortésmente preguntas no relacionadas
- ✅ Responder siempre en español

## 📝 Prompts del Sistema

Los prompts están documentados en `src/prompts.py` para facilitar su inclusión en la memoria del TFM. Incluyen:

- `SYSTEM_PROMPT`: Instrucciones principales del asistente
- `REJECTION_PROMPT`: Mensaje para preguntas no relacionadas
- `get_data_context_prompt()`: Función para formatear el contexto de datos

## 🛠️ Desarrollo

### Estructura de datos

El archivo `data/tourism_data.json` contiene registros semanales con:

- Código y nombre de isla
- Fecha de inicio de semana
- Total de turistas
- Pasajeros internacionales/domésticos
- País de origen más común
- Tasa de ocupación hotelera
- Tarifa diaria promedio
- Ingresos y gastos
- Duración de estancia
- Eventos y asistencia

### Extensibilidad

El sistema está diseñado para ser fácilmente extensible:

- Añadir nuevos filtros en `rag.py`
- Modificar prompts en `prompts.py`
- Ajustar configuración en `config.py`
- Implementar rate limiting
- Añadir logging y monitoreo

## 📚 Documentación API

Una vez la API esté corriendo, accede a:

- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

## 🐛 Troubleshooting

### Error: "ANTHROPIC_API_KEY no está configurada"
- Verifica que el archivo `.env` existe y contiene la clave

### Error: "Sistema RAG no inicializado"
- Verifica que el archivo `data/tourism_data.json` existe y contiene datos válidos
- Revisa los logs para errores de carga del archivo

### Error 401: "API Key inválida"
- Verifica que el header `X-API-Key` está presente
- Confirma que el valor coincide con `MASTER_API_KEY` en el `.env`

## 📄 Licencia

Este proyecto es parte de un Trabajo Final de Máster (TFM).

## 👤 Autor

Desarrollado para el TFM - Analítica de Turismo en Canarias con IA

---

**Nota**: Este README incluye toda la información necesaria para la documentación del TFM, incluyendo decisiones de diseño, arquitectura y ejemplos de uso.
