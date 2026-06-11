# Mini-Turnitin — Curso PLN

Sistema de detección de similitud y texto generado por IA para documentos académicos en español.

## Estructura

```
mini_turnitin/
├── data/
│   └── corpus/          ← coloca aquí los documentos de referencia (.txt)
├── src/
│   ├── preprocess.py    ← limpieza, tokenización, lematización (spaCy)
│   ├── lexical.py       ← TF-IDF + coseno, MinHash
│   ├── semantic.py      ← Sentence-BERT multilingüe, detección de paráfrasis
│   ├── detector_ia.py   ← perplejidad + burstiness (GPT-2 español)
│   └── report.py        ← generación de reporte HTML
├── main.py              ← motor principal + CLI
├── app.py               ← interfaz Streamlit
└── requirements.txt
```

## Instalación

```bash
pip install -r requirements.txt
python -m spacy download es_core_news_sm
```

Dependencias opcionales (para funciones avanzadas):

- **Perplexity con GPT-2**: si quieres habilitar el cálculo de perplexity con GPT-2 instala `transformers` y `torch`:

```bash
pip install transformers torch
```

Después de instalarlas, activa la opción "Usar GPT-2 para perplexity" en la interfaz.

## Uso

### Interfaz web (recomendado)

```bash
streamlit run app.py
```

### Línea de comandos

```bash
python main.py trabajo_alumno.txt referencia.txt
# Con reporte HTML:
python main.py trabajo_alumno.txt referencia.txt --reporte mi_reporte.html
# Sin análisis IA (más rápido):
python main.py trabajo_alumno.txt referencia.txt --no-ia
```

### Como librería

```python
from main import analyze
from src.report import generate_report

result = analyze(texto_alumno, texto_referencia, check_ia=True)
print(result["score_final"], result["nivel"])
generate_report(result, "reporte.html")
```

## Interpretación de scores

| Score final | Nivel  | Interpretación                          |
|-------------|--------|-----------------------------------------|
| > 75%       | Alto   | Alta probabilidad de plagio o paráfrasis|
| 45–75%      | Medio  | Similitud moderada, revisar fragmentos  |
| < 45%       | Bajo   | Textos sustancialmente diferentes       |

## Hoja de ruta del curso

| Semana | Tema                   | Módulo                  |
|--------|------------------------|-------------------------|
| 1      | Preprocesamiento       | `preprocess.py`         |
| 2      | Representación vectorial| `lexical.py`           |
| 3      | Word embeddings        | `semantic.py`           |
| 4      | Modelos de lenguaje    | `detector_ia.py`        |
| 5      | Sistemas completos     | `main.py` + `app.py`    |
