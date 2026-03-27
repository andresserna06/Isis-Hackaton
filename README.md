# 🤖 Isis — Marketing Co-piloto
**CloudLabs Learning · Hackathon Talento Tech 2026**

Isis es un co-piloto de Marketing impulsado por Inteligencia Artificial, diseñado para que el equipo de Marketing de CloudLabs pueda consultar, entender y visualizar patrones de comportamiento web de forma simple e intuitiva, sin necesidad de conocimientos técnicos.

---

## 📋 Tabla de contenidos

1. [Descripción del proyecto](#descripción-del-proyecto)
2. [Arquitectura de la solución](#arquitectura-de-la-solución)
3. [Estructura de archivos](#estructura-de-archivos)
4. [Requisitos](#requisitos)
5. [Instalación y ejecución](#instalación-y-ejecución)
6. [Datasets](#datasets)
7. [Motor analítico](#motor-analítico)
8. [Interfaz de usuario](#interfaz-de-usuario)
9. [Co-piloto conversacional (Isis)](#co-piloto-conversacional-isis)
10. [Insights generados](#insights-generados)
11. [Semáforo de salud web](#semáforo-de-salud-web)
12. [Equipo](#equipo)

---

## 📌 Descripción del proyecto

CloudLabs captura miles de interacciones diarias mediante Microsoft Clarity (visitas, clics, puntos de abandono). El problema no es la falta de datos — es la fricción para convertirlos en decisiones estratégicas oportunas.

**Isis** resuelve esto con una solución end-to-end que integra:

- **Motor analítico**: Procesa los datasets de Microsoft Clarity y calcula métricas clave.
- **Dashboard visual**: Gráficas interactivas con filtros por URL para análisis por página.
- **Co-piloto con LLM**: El usuario pregunta en lenguaje natural y recibe datos crudos + interpretación clara para el negocio.

---

## 🏗️ Arquitectura de la solución

```
┌─────────────────────────────────────────────────────────┐
│                    USUARIO DE MARKETING                  │
│              (Pregunta en lenguaje natural)              │
└───────────────────────┬─────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│              INTERFAZ GRADIO (Isis)                      │
│         Dashboard  ◄──────────►  Chat                   │
└──────────────┬──────────────────────┬───────────────────┘
               │                      │
               ▼                      ▼
┌──────────────────────┐   ┌──────────────────────────────┐
│   MOTOR ANALÍTICO    │   │         LLM (Groq)            │
│   Python / Pandas    │   │    llama-3.1-8b-instant       │
└──────────┬───────────┘   └──────────────────────────────┘
           │
    ┌──────┴───────┐
    ▼              ▼
┌────────┐  ┌──────────┐
│Recordi-│  │ Metrics  │
│  ngs   │  │  Clean   │
└────────┘  └──────────┘
```

**Flujo de una consulta:**
1. El usuario escribe una pregunta en lenguaje natural
2. El motor analítico calcula las métricas relevantes desde los datasets
3. El LLM (Groq / Llama 3.1) recibe los datos y genera una interpretación clara
4. La interfaz muestra: datos crudos + interpretación para el negocio

---

## 📁 Estructura de archivos

```
proyecto/
│
├── solución.ipynb                  # Notebook principal con todo el código
├── 1_Data_Recordings_CLEAN.csv     # Dataset de sesiones por usuario (limpio)
├── Data_Metrics_clean.csv          # Dataset de métricas agregadas (limpio)
└── README.md                       # Este archivo
```

---

## ⚙️ Requisitos

### Dependencias Python
```
gradio
groq
pandas
tabulate
plotly
```

### API Key requerida
- **Groq API Key** (gratuita): https://console.groq.com

### Entorno de ejecución
- Google Colab (recomendado)
- Python 3.10+

---

## 🚀 Instalación y ejecución

### Paso 1 — Instalar dependencias
```python
!pip install gradio groq pandas tabulate plotly -q
```

### Paso 2 — Configurar API Key en Colab
1. Ir al ícono 🔑 en el panel izquierdo de Colab
2. Agregar un secret con nombre `GROQ_API_KEY` y el valor de tu key
3. Activar el toggle "Notebook access"

```python
from google.colab import userdata
from groq import Groq

GROQ_API_KEY = userdata.get('GROQ_API_KEY')
client = Groq(api_key=GROQ_API_KEY)
```

### Paso 3 — Subir los datasets
En el panel izquierdo de Colab, ir a la carpeta 📁 y subir:
- `1_Data_Recordings_CLEAN.csv`
- `Data_Metrics_clean.csv`

### Paso 4 — Ejecutar las celdas en orden
| Celda | Descripción |
|-------|-------------|
| Celda 1 | Instalación de dependencias |
| Celda 2 | Configuración de API Key y prueba de conexión |
| Celda 3 | Carga de datasets y motor analítico |
| Celda 4 | Configuración del LLM y prompts |
| Celda 5 | Funciones del co-piloto y cálculo de datos |
| Celda 6 | Interfaz Gradio (Dashboard + Chat) |

### Paso 5 — Acceder a la interfaz
Al ejecutar la Celda 6 se generará un link público:
```
Running on public URL: https://xxxx.gradio.live
```
Compartir ese link con el equipo de Marketing para acceso inmediato.

---

## 📊 Datasets

### 1_Data_Recordings_CLEAN.csv
Contiene datos granulares de cada sesión de usuario. **67,356 registros, 31 columnas.**

| Columna | Descripción |
|---------|-------------|
| `fecha` / `hora` | Momento exacto de la sesión |
| `direccion_url_entrada` | Página por donde ingresó el usuario |
| `direccion_url_salida` | Página donde terminó la sesión |
| `duracion_sesion_segundos` | Tiempo total de la sesión |
| `dispositivo` | PC, Mobile, Tablet |
| `sistema_operativo` | Windows, Android, iOS, etc. |
| `pais` | País de origen |
| `clics_sesion` | Total de clics en la sesión |
| `tiempo_por_pagina` | Segundos promedio por página |
| `posible_frustracion` | 1 si se detectó frustración, 0 si no |
| `standarized_engagement_score` | Score de engagement estandarizado |
| `abandono_rapido` | 1 si el usuario salió en menos de 10 segundos |
| `trafico_externo` | 1 si viene de otro sitio, 0 si es directo |

### Data_Metrics_clean.csv
Contiene datos agregados por URL, dispositivo y sistema operativo. **20,595 registros, 16 columnas.**

| Columna | Descripción |
|---------|-------------|
| `Url` | URL analizada |
| `Device` / `OS` | Segmentación técnica |
| `metricName` | Tipo de métrica (DeadClick, RageClick, etc.) |
| `sessionsCount` | Número de sesiones en ese segmento |
| `pagesViews` | Vistas de página |
| `averageScrollDepth` | Profundidad de scroll promedio |

**Proceso de limpieza aplicado:**
- Eliminación de nulos en columnas esenciales (`sessionsCount`, `Url`, `Device`, `OS`, `metricName`)
- Eliminación de duplicados
- Detección y remoción de outliers con método IQR
- Codificación de variables categóricas

---

## 🔧 Motor analítico

El motor analítico es el núcleo de la solución. Calcula las siguientes métricas:

### Métricas obligatorias
| Función | Descripción |
|---------|-------------|
| `get_top_pages()` | Ranking de páginas con más visitas |
| `get_abandono()` | Páginas con mayor tasa de salida |
| `get_duracion_promedio()` | Promedio, máximo y mínimo de segundos por sesión |
| `get_dispositivos()` | Distribución porcentual por dispositivo |
| `get_frustracion()` | % de sesiones con frustración detectada |
| `get_engagement()` | Score de engagement promedio (en %) |

### Insights adicionales propuestos por el equipo
| Función | Descripción | Justificación |
|---------|-------------|---------------|
| `get_abandono_rapido()` | % de usuarios que salen en menos de 10 segundos | Indica si el contenido de entrada no genera interés inmediato |
| `get_trafico_externo()` | % de tráfico externo vs directo | Evalúa efectividad de campañas externas |
| `get_trafico_por_hora()` | Horas del día con más tráfico | Permite programar publicaciones en horarios de mayor audiencia |
| `get_segundos_por_pagina()` | Tiempo promedio por página | Identifica qué páginas retienen más la atención |
| `get_top_paises()` | Top 10 países con más sesiones | Permite entender la distribución geográfica del tráfico |

---

## 🖥️ Interfaz de usuario

La interfaz tiene dos pestañas principales:

### 📊 Tab Dashboard

**Filtro global por URL**: Selector que permite filtrar el semáforo y el análisis de abandono por página de entrada específica.

**Semáforo de salud web**: Diagnóstico visual con 5 indicadores clave que cambian de color según umbrales:
- Engagement, Frustración, Abandono rápido, Tráfico externo, Segundos por página
- 🟢 Bueno / 🟡 Moderado / 🔴 Malo — se actualiza dinámicamente con el filtro de URL

**Diagnóstico de Isis**: Botón que envía los indicadores del semáforo al LLM y genera un análisis ejecutivo con el estado general del sitio y dos acciones concretas para la semana.

**KPIs principales**: 4 tarjetas con duración promedio, engagement, frustración y página top.

**Gráficas interactivas**:
- Páginas más visitadas (barras horizontales)
- Páginas con mayor abandono (barras horizontales)
- Distribución por dispositivo (pie chart)
- Gauge de engagement y frustración
- Mapa coroplético de tráfico por país
- Tendencia de sesiones en el tiempo con línea de promedio
- Frustración por dispositivo (barras verticales)
- Mapa de calor de tráfico por día y hora
- Flujo de navegación Sankey
- Segmentación de usuarios por comportamiento
- Análisis de abandono filtrable por URL de entrada

### 🤖 Tab Chat con Isis

Interfaz conversacional donde el usuario escribe preguntas en español y recibe:

1. **Datos crudos**: Métricas exactas calculadas desde los datasets
2. **Interpretación**: Análisis en lenguaje simple orientado a decisiones de negocio
3. **Recomendación**: Acción concreta que el equipo puede ejecutar

**Preguntas de ejemplo incluidas** para facilitar el uso por perfiles no técnicos.

---

## 💬 Co-piloto conversacional (Isis)

### Modelo LLM
- **Proveedor**: Groq (gratuito)
- **Modelo**: `llama-3.1-8b-instant`
- **Tiempo de respuesta**: 3-7 segundos por consulta

### Cómo funciona
Los datos se precalculan una sola vez al iniciar la aplicación (`DATOS_GLOBALES`) y se incluyen en cada consulta al LLM. Esto garantiza respuestas rápidas y precisas basadas en los datos reales.

### Prompt principal
Isis tiene un perfil de experta en Marketing Digital con más de 10 años de experiencia. Sus respuestas siguen siempre este formato:

```
📊 DATOS
[Métricas relevantes]

🧠 INTERPRETACIÓN
[Qué significan para el negocio]

💡 RECOMENDACIÓN
[Acción concreta]
```

### Memoria conversacional
El historial de la conversación se incluye en cada llamada al LLM, permitiendo preguntas de seguimiento y contexto acumulado durante la sesión.

---

## 🚦 Semáforo de salud web

| Indicador | 🟢 Bueno | 🟡 Moderado | 🔴 Malo |
|-----------|----------|-------------|---------|
| Engagement | ≥ 5% | ≥ 1% | < 1% |
| Frustración | ≤ 20% | ≤ 40% | > 40% |
| Abandono rápido | ≤ 30% | ≤ 50% | > 50% |
| Tráfico externo | ≥ 40% | ≥ 20% | < 20% |
| Seg. por página | ≥ 120s | ≥ 30s | < 30s |

El semáforo se actualiza dinámicamente al filtrar por URL, mostrando el diagnóstico específico de cada página.

---

## 👥 Equipo

Desarrollado en el marco del **Hackathon Talento Tech 2026** — Programa de Inteligencia Artificial.

**Empresa retante**: CloudLabs Learning  
**Reto**: Marketing Copilot basado en comportamientos web  
**Tecnologías**: Python, Pandas, Gradio, Plotly, Groq, Llama 3.1  
**Datos**: Microsoft Clarity — 67,356 sesiones reales

---

*Isis · Powered by Groq · CloudLabs Learning © 2026*