# Ejercicio 2 · Análisis de reseñas de videojuegos con LLMs

Pipeline de dos pasos con LLMs para analizar reseñas de videojuegos de Steam:

1. **LLM 1 · Filtro (`gemini-3.5-flash-lite`, Google):** descarta las reseñas poco informativas (spam, opiniones genéricas). Lee un extracto de cada reseña y relee completas solo las dudosas (cascada).
2. **LLM 2 · Extracción (`gpt-4.1-mini`, OpenAI):** extrae información estructurada de las reseñas relevantes: juego, sentimiento, valoración por aspectos, dificultad, problemas técnicos y recomendación.

La extracción se planteó inicialmente con `gemini-3.6-flash`, pero su capa gratuita (20 peticiones diarias) se agotó durante la ejecución y se cambió a OpenAI. La incidencia y todas las decisiones (modelos, prompts, batching, tokens y rate limits) están documentadas en el notebook.

## Estructura

```
├── Ejercicio_2.ipynb       Notebook principal
├── data/                   Datasets de entrada
├── cache/                  Respuestas de los LLMs guardadas por lote
├── outputs/                Resultados finales (CSV y JSON)
├── requirements.txt
└── README.md
```

## Cómo ejecutarlo

1. Crear y activar un entorno virtual:
   ```bash
   python -m venv .venv
   .venv\Scripts\activate        # Windows
   source .venv/bin/activate     # macOS / Linux
   ```
2. Instalar dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Crear un archivo `.env` en la raíz con las dos API keys:
   ```
   GOOGLE_API_KEY=tu_clave_de_google_ai_studio
   OPENAI_API_KEY=tu_clave_de_openai
   ```
4. Abrir `Ejercicio_2.ipynb` y ejecutar todas las celdas.

> La carpeta `cache/` contiene las respuestas ya obtenidas de los modelos. Si existe, el notebook las reutiliza y no repite llamadas a las APIs. Para regenerar los resultados desde cero, basta con borrarla.

## Resultados

- `outputs/paso2_resenas_filtradas.csv`: reseñas que superan el filtro del LLM 1.
- `outputs/analisis_videojuegos_final.csv` / `.json`: resultado estructurado final.
- `outputs/consumo_llamadas.csv`: tokens y tiempo de cada llamada.
