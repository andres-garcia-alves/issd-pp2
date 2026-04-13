# Plan de Proyecto — PP2: Detección de Patrones en Accidentes en la Vía Pública

**Materia:** Práctica Profesionalizante 2  
**Tecnicatura:** Inteligencia Artificial y Ciencia de Datos — ISSD  
**Integrantes:** Garcia Alves, Andrés · Vega, Horacio Facundo  
**Profesor:** Paredes Rojas, Julio  
**Fecha:** Abril 2026  

---

## Datasets

| Archivo | Separador | Descripción |
|---|---|---|
| `dataset siniestros_viales_hechos.csv` | `;` | Un registro por siniestro (2019-2024). Incluye ubicación, fecha/hora, participantes, gravedad. |
| `dataset siniestros_viales_victimas.csv` | `;` | Un registro por víctima. Incluye perfil demográfico, rol, gravedad, fecha de fallecimiento. |

**Fuente:** Observatorio de Movilidad y Seguridad Vial — GCBA  
**URLs:** https://data.buenosaires.gob.ar/dataset/victimas-siniestros-viales

---

## Decisiones de Diseño

- **Entorno de ejecución:** VS Code local + Google Colab (rutas adaptables automáticamente)
- **Visualización principal:** Plotly (interactivo)
- **Tecnologías obligatorias:** Gradio + Ngrok + MLFlow (~1 línea de tracking)

---

## Estructura del Notebook

### Fase 0 — Encabezado *(celdas 1-2 — existentes, no modificar)*

### Fase 1 — Abstract e Hipótesis
- Descripción del proyecto, objetivo, datasets, metodología breve

### Fase 2 — Preguntas e Hipótesis de Investigación *(nueva celda 4)*
1. ¿En qué franjas horarias y días se concentran los siniestros graves/mortales?
2. ¿Qué comunas presentan mayor incidencia y de qué gravedad?
3. ¿Cuál es la combinación de participantes más peligrosa? (MOTO-AUTO, PEATON-AUTO, etc.)
4. ¿Cómo evolucionó la siniestralidad 2019-2024? (efecto COVID 2020)
5. ¿Qué perfil demográfico (edad, sexo) tienen las víctimas mortales vs. leves?
6. ¿Qué tipo de vía (autopista, avenida, calle) concentra más siniestros graves?

### Fase 3 — Preprocesamiento

| Paso | Descripción |
|---|---|
| 3.1 | Importación de librerías (numpy, pandas, plotly, sklearn, mlflow, gradio) |
| 3.2 | Detección de entorno (Colab vs. local) y configuración de rutas |
| 3.3 | Carga de los dos datasets con `sep=';'` |
| 3.4 | Inspección inicial: shape, dtypes, head(3), describe() |
| 3.5 | Validación de nulos (identificar y tratar — imputar o mantener 'SD') |
| 3.6 | Verificación de duplicados por `id_siniestro` / `id_hecho` |
| 3.7 | Conversión de tipos: `fecha→datetime`, `edad→int`, `hora→int` |
| 3.8 | Feature engineering (ver detalle abajo) |
| 3.9 | Merge hechos + víctimas por `id_siniestro` / `id_hecho` |

**Variables construidas:**

| Variable | Tipo | Descripción |
|---|---|---|
| `es_fin_de_semana` | bool | True si el siniestro ocurrió sábado o domingo |
| `franja_del_dia` | category | Madrugada (0-5), Mañana (6-11), Tarde (12-17), Noche (18-23) |
| `edad_grupo` | category | Bins: <18, 18-30, 30-60, >60 |
| `es_mortal` | bool | True si `gravedad == 'MORTAL'` |
| `tiempo_hasta_fallecimiento` | int (días) | Solo para víctimas mortales: días entre siniestro y fallecimiento |

### Fase 4 — EDA (Análisis Exploratorio)

| Pregunta | Visualizaciones |
|---|---|
| P1: Franjas horarias | Heatmap hora×día de la semana; barplot por franja; lineplot por hora |
| P2: Comunas | Barplot por comuna (coloreado por gravedad); scatter map lat/lon |
| P3: Participantes | Treemap o barplot de combinaciones víctima-contraparte |
| P4: Evolución temporal | Lineplot anual 2019-2024; barplot mensual; anotación COVID |
| P5: Perfil demográfico | Violin/box de edad por gravedad; stacked bar por sexo |
| P6: Tipo de vía | Stacked bar gravedad × tipo de vía; pie de distribución |

### Fase 5 — Modelo de ML *(a definir en sesión futura)*

- **Candidato:** Clasificación de gravedad (LEVE / GRAVE / MORTAL)
- **Algoritmo:** RandomForest o XGBoost (según performance)
- **Tracking:** MLFlow — log de métricas del experimento (~1 línea)

### Fase 6 — Demo Interactiva

- **Gradio:** Interfaz web con inputs (hora, día, comuna, participante, tipo de vía) → output (nivel de gravedad predicho)
- **Ngrok:** Expone la interfaz como URL pública para compartir

### Fase 7 — Segunda Etapa: Artículo Académico *(sesión futura)*

Formato HTML embebido en el notebook:
- Portada formal
- Introducción
- Marco teórico (seguridad vial, CABA, metodología OMSV)
- Metodología del proyecto
- Resultados (hallazgos EDA + modelo)
- Conclusiones
- 6 referencias APA con links verificables

---

## Criterios de Calidad

- Notebook ejecutable completo sin errores (Restart & Run All)
- Rutas compatibles con local y Google Colab
- Celdas de código bien comentadas
- Hipótesis claramente formuladas y respondidas con evidencia visual
- Nivel de trabajo apto para revistas institucionales con referato (Q3/Q4)

---

## Historial de Decisiones

| Fecha | Decisión |
|---|---|
| Abr 2026 | Visualización principal: Plotly (mayor interactividad vs. Bokeh/Altair del PP1) |
| Abr 2026 | Modelo ML: TBD (clasificación de gravedad como candidato principal) |
| Abr 2026 | Artículo en español (para apuntar a revistas institucionales LATAM) |
