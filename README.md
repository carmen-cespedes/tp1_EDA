# TP1 — EDA

Repositorio del TP, centrado en un análisis exploratorio de datos sobre patrones de sueño y rendimiento diario.

---

## Objetivo del trabajo

Desarrollar un Análisis Exploratorio de Datos (EDA) sobre un dataset real, aplicando técnicas de limpieza, transformación y visualización para responder un conjunto de hipótesis previamente definidas y extraer conclusiones relevantes sobre los patrones de sueño de la población.

---

## Contexto del dataset

**Nombre:** Sleep Health & Daily Performance Dataset  
**Fuente:** [Kaggle](https://www.kaggle.com/datasets/mohankrishnathalla/sleep-health-and-daily-performance-dataset)  
**Tamaño:** 100.000 registros, 32 columnas  
**Última actualización:** mayo 2026

El dataset contiene mediciones de calidad y composición del sueño de 100.000 individuos pertenecientes a 15 países y 12 ocupaciones distintas. Incluye métricas objetivas (duración, porcentaje REM, porcentaje de sueño profundo, latencia, episodios de despertar), subjetivas (calidad percibida, sensación de descanso) y variables de contexto (hábitos previos al sueño, estrés, actividad física, condiciones ambientales).

---

## Diccionario de datos

### Variables originales del dataset

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `person_id` | Identificador | ID único del individuo |
| `age` | Numérica continua | Edad en años (rango: 18-69) |
| `gender` | Categórica | Género autoidentificado |
| `occupation` | Categórica | Ocupación (12 categorías: Doctor, Nurse, Lawyer, Driver, Student, Software Engineer, Manager, Sales, Teacher, Homemaker, Freelancer, Retired) |
| `bmi` | Numérica continua | Índice de masa corporal (kg/m²) |
| `country` | Categórica | País de residencia (15 países) |
| `sleep_duration_hrs` | Numérica continua | Duración total del sueño en horas |
| `sleep_quality_score` | Numérica ordinal | Calidad subjetiva del sueño (1 = muy mala, 10 = excelente) |
| `rem_percentage` | Numérica continua | Porcentaje de sueño en fase REM |
| `deep_sleep_percentage` | Numérica continua | Porcentaje de sueño profundo (ondas lentas) |
| `sleep_latency_mins` | Numérica continua | Minutos hasta quedarse dormido |
| `wake_episodes_per_night` | Numérica discreta | Cantidad de despertares nocturnos |
| `caffeine_mg_before_bed` | Numérica continua | Miligramos de cafeína consumidos antes de dormir |
| `alcohol_units_before_bed` | Numérica continua | Unidades de alcohol consumidas antes de dormir |
| `screen_time_before_bed_mins` | Numérica continua | Minutos de pantalla antes de acostarse |
| `exercise_day` | Binaria (0/1) | Si realizó ejercicio ese día |
| `steps_that_day` | Numérica continua | Cantidad de pasos registrados ese día |
| `nap_duration_mins` | Numérica continua | Duración de la siesta en minutos |
| `stress_score` | Numérica continua | Nivel de estrés percibido |
| `work_hours_that_day` | Numérica continua | Horas trabajadas ese día |
| `chronotype` | Categórica | Cronotipo (Morning / Evening / Intermediate) |
| `mental_health_condition` | Categórica | Condición de salud mental reportada |
| `heart_rate_resting_bpm` | Numérica continua | Frecuencia cardíaca en reposo (ppm) |
| `sleep_aid_used` | Binaria (0/1) | Si utilizó algún auxiliar para dormir |
| `shift_work` | Binaria (0/1) | Si trabaja en turnos rotativos |
| `room_temperature_celsius` | Numérica continua | Temperatura del cuarto al dormir (°C) |
| `weekend_sleep_diff_hrs` | Numérica continua | Diferencia de horas de sueño entre semana y fin de semana |
| `season` | Categórica | Estación del año |
| `day_type` | Categórica | Tipo de día (weekday / weekend) |
| `cognitive_performance_score` | Numérica continua | Puntuación de rendimiento cognitivo |
| `sleep_disorder_risk` | Categórica | Nivel de riesgo de trastorno del sueño |
| `felt_rested` | Binaria (0/1) | Si la persona se sintió descansada al despertar |

### Variables derivadas (creadas en el análisis)

| Columna | Descripción |
|---------|-------------|
| `deep_sleep_min` | Minutos absolutos de sueño profundo (`sleep_duration_hrs × 60 × deep_sleep_percentage / 100`) |
| `rem_sleep_min` | Minutos absolutos de sueño REM |
| `age_group` | Grupo etario: 18-25, 26-40, 41-60, 60+ |
| `bmi_category` | Categoría de BMI según OMS: Bajo peso, Normal, Sobrepeso, Obesidad |
| `sleep_category` | Categoría de duración: Insuficiente (<6h), Recomendado (6-9h), Excesivo (>9h) |
| `demanda_laboral` | Nivel de demanda ocupacional: Alta, Media, Baja |
| `tipo_durmiente` | Tipología de durmiente: Eficiente, Óptimo, Privado, Ineficiente, Intermedio |

---

## Hipótesis planteadas

1. **H1:** ¿Existe correlación positiva entre la duración del sueño y la calidad subjetiva percibida?
2. **H2:** ¿Las ocupaciones de alta demanda (Doctor, Nurse, Manager) muestran peores indicadores de sueño que las de baja demanda (Teacher, Retired, Homemaker)?
3. **H3:** ¿El BMI elevado se asocia con menor porcentaje de sueño profundo?
4. **H4:** ¿La edad correlaciona negativamente con el porcentaje de sueño REM?
5. **H5:** ¿Existen "durmientes eficientes" (poca duración pero alto % de sueño profundo y buena calidad)? ¿Qué características los distinguen?

---

## Metodología aplicada

El análisis se desarrolló en Python (Jupyter Notebook) y siguió los siguientes pasos:

1. ###Carga e inspección inicial: 
verificación de dimensiones, tipos de datos y estadísticos descriptivos.
2. ###Limpieza de datos:
detección de nulos, duplicados, validación de rangos lógicos y coherencia interna (verificación de que REM% + Deep% ≤ 100%).
3. ### 3. La paradoja médica: el caso de los doctores (sub-análisis de H2) 
minutos absolutos de sueño, grupos etarios, categorías de BMI, demanda laboral y tipología de durmientes.
4. ### 4. El BMI tiene un impacto direccional pero débil sobre el sueño profundo (H3)
   - 3 histogramas con KDE para variables numéricas clave
   - Detección de outliers mediante el método del rango intercuartílico (IQR)
   - Matriz de correlación entre variables numéricas
5.### 5. La edad no predice el porcentaje de sueño REM en este dataset (H4)
combinación de visualizaciones específicas (boxplots, scatterplots) con tests estadísticos (Pearson, ANOVA, Tukey HSD, t-test, Chi²).

---

## Resultados por hipótesis

| # | Hipótesis | Test estadístico | Resultado | Conclusión |
|---|-----------|------------------|-----------|------------|
| H1 | Correlación duración ↔ calidad | Pearson | r = 0.6465, p < 0.001 | **Confirmada** — correlación positiva fuerte (r² = 0.42) |
| H2 | Demanda laboral influye en el sueño | ANOVA + Tukey HSD | F = 4440.68, p < 0.001 | **Confirmada** — gradiente claro, diferencia > 1 punto entre extremos |
| H3 | BMI inversamente proporcional al sueño profundo | Pearson + ANOVA | r = −0.0472, F = 63.19, p < 0.001 | **Confirmada parcialmente** — dirección correcta, magnitud débil (r² = 0.002) |
| H4 | Edad disminuye % REM | Pearson | r = 0.0016, p = 0.60 | **Rechazada** — sin correlación significativa |
| H5 | Existen durmientes eficientes | t-test + Chi² | Múltiples tests, p < 0.001 | **Confirmada** — 287 eficientes identificados, perfil distintivo |

---

## Hallazgos relevantes

### 1. ¿Existe correlación positiva entre la duración del sueño y la calidad subjetiva percibida?
### La duración del sueño es un predictor relevante de la calidad percibida (H1)

El análisis revela una correlación positiva fuerte entre las horas dormidas y la calidad subjetiva (r = 0.65, p < 0.001). La duración del sueño explica aproximadamente el **42% de la variabilidad** en la calidad percibida (r² ≈ 0.42), confirmando que es un factor importante pero no único: el 58% restante depende de otros factores como la composición del sueño, el estrés y los hábitos previos al descanso.

### 2. ¿Las ocupaciones de alta demanda (Doctor, Nurse, Manager) muestran peores indicadores de sueño que las de baja demanda (Teacher, Retired, Homemaker)?
### La demanda ocupacional impacta significativamente en el sueño (H2)

Existe un gradiente claro y estadísticamente significativo entre los tres niveles de demanda laboral:

| Nivel | Calidad media |
|-------|--------------|
| Baja demanda (Teacher, Retired, Homemaker, Freelancer, Student) | 5.35 |
| Demanda media (Software Engineer, Lawyer, Sales, Driver) | 4.67 |
| Alta demanda (Doctor, Nurse, Manager) | 4.33 |

La diferencia entre los extremos supera **1 punto** en una escala de 1 a 10. El test post-hoc de Tukey confirma diferencias significativas en las tres comparaciones por pares (p < 0.001).

### 3. Creación de variables derivadas:** minutos absolutos de sueño, grupos etarios, categorías de BMI, demanda laboral y tipología de durmientes.
### La paradoja médica: el caso de los doctores (sub-análisis de H2)

Los doctores (N = 7.868) presentan un patrón especialmente preocupante:

- **Duración promedio: 5.93 hrs** (vs. 6.42 hrs del dataset general)
- **El 53.90% duerme menos de 6 hrs por noche** (categoría "insuficiente")
- Solo el 17.32% alcanza el rango recomendado (7-9 hrs)
- Diferencia altamente significativa respecto al resto (t = −36.07, p < 0.001)

Los profesionales con mayor conocimiento sobre las consecuencias del mal sueño son sistemáticamente quienes peor duermen, debido a factores estructurales del sistema sanitario (guardias rotativas, atención 24/7, residencias intensivas).

### 4. **EDA general:**
   - 3 histogramas con KDE para variables numéricas clave
   - Detección de outliers mediante el método del rango intercuartílico (IQR)
   - Matriz de correlación entre variables numéricas
### El BMI tiene un impacto direccional pero débil sobre el sueño profundo (H3)

Se detectó una asociación negativa entre BMI y % de sueño profundo (r = −0.047, p < 0.001), confirmando la dirección predicha por la literatura clínica. Sin embargo, la **magnitud del efecto es muy débil**: el BMI explica apenas el **0.2% de la variabilidad** del sueño profundo (r² ≈ 0.002).

Este resultado ilustra una distinción metodológica fundamental: **significancia estadística ≠ relevancia práctica**. Con muestras grandes, casi cualquier diferencia se detecta como significativa; reportar el tamaño del efecto (r, r²) es tan importante como reportar el p-valor.

### 5. **Análisis por hipótesis:** combinación de visualizaciones específicas (boxplots, scatterplots) con tests estadísticos (Pearson, ANOVA, Tukey HSD, t-test, Chi²).
### La edad no predice el porcentaje de sueño REM en este dataset (H4)

No se observa relación entre edad y % REM (r = 0.0016, p = 0.60). La hipótesis fue rechazada, posiblemente por el carácter sintético del dataset, el rango etario limitado (18-69 años) o la alta variabilidad individual. Este resultado refuerza la importancia de **verificar empíricamente las relaciones esperadas** en lugar de asumirlas.

### 6. gráfico de barras (calidad por país), gráfico de torta (distribución de durmientes), gráfico de líneas (evolución por edad).
### Existen durmientes eficientes con perfil identificable (H5)

Se identificó un grupo de **287 durmientes eficientes** (0.29% del dataset): personas que con menos de 5.81 horas logran un % de sueño profundo > 22.1% y calidad subjetiva > 5.6. Son más jóvenes (edad media: 29.2 años), tienen un BMI ligeramente menor y su sueño profundo en minutos es significativamente mayor que el de los durmientes "privados" (t = 61.47, p < 0.001).

---

## Conclusión final

1. **La duración importa, pero no en su totalidad.** Explica el 42% de la calidad percibida; el resto responde a la composición del sueño, el estrés y otros factores.
2. **El trabajo configura el sueño.** Existe un gradiente claro entre niveles de demanda ocupacional, con los doctores como caso paradigmático de déficit crónico estructural.
3. **La composición puede compensar la duración.** El grupo de "durmientes eficientes" logra descanso óptimo con pocas horas, sugiriendo que la calidad del sueño profundo es un recurso individual valioso.
4. **Significancia ≠ Relevancia.** El caso del BMI demuestra que con muestras grandes se detectan efectos estadísticamente significativos pero prácticamente irrelevantes.
5. **Verificar siempre los supuestos.** El rechazo de H4 demuestra la importancia de no asumir relaciones esperadas sin contrastarlas con los datos.

---

## Limitaciones del estudio

- **Carácter transversal:** una sola medición por persona; no permite inferir causalidad ni efectos acumulados a largo plazo.
- **Posible origen sintético:** el rechazo de H4 y la baja magnitud de H3 sugieren que el dataset podría no replicar fielmente todos los patrones biológicos documentados en la literatura médica.
- **Calidad subjetiva auto-reportada:** `sleep_quality_score` está sujeto a sesgo de percepción y deseabilidad social.
- **Sin distinción entre siestas y sueño nocturno:** `sleep_duration_hrs` agrupa todo el sueño del día.
- **Variables no exploradas en profundidad:** `caffeine_mg_before_bed`, `alcohol_units_before_bed`, `stress_score`, `screen_time_before_bed_mins` y `nap_duration_mins`, entre otras, podrían enriquecer análisis futuros.

---

## Líneas futuras de investigación

- Analizar el impacto del consumo de cafeína, alcohol y tiempo de pantalla antes de dormir sobre la composición del sueño.
- Estudiar la relación entre `stress_score` y la arquitectura del sueño.
- Explorar `nap_duration_mins` para evaluar si los durmientes eficientes compensan con siestas.
- Aplicar clustering no supervisado (K-means, DBSCAN) para validar la tipología de durmientes propuesta en H5.
- Comparar patrones de sueño entre países para identificar diferencias culturales o sistémicas.
- Acceder a estudios longitudinales reales (UK Biobank, NHANES) para medir efectos acumulados a largo plazo.

---

## Estructura del repositorio

```
tp1_EDA/
├── dataset.csv               # Dataset original (100.000 registros, 32 columnas)
├── sleep_health_limpio.csv   # Dataset con variables derivadas (generado al correr el notebook)
├── analisis.ipynb            # Notebook principal con el análisis completo
└── README.md                 # Este archivo
```
