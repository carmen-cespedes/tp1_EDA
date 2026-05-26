Repositorio del TP, centrado en un análisis exploratorio de datos sobre patrones de sueño y rendimiento diario.

---

## Objetivo del trabajo

Desarrollar un Análisis Exploratorio de Datos (EDA) sobre un dataset real, aplicando técnicas de limpieza, transformación y visualización para responder un conjunto de hipótesis previamente definidas y extraer conclusiones relevantes sobre los patrones de sueño de la población.

---

## Contexto del dataset

**Nombre:** Sleep Health & Daily Performance Dataset
**Fuente:** [Kaggle](https://www.kaggle.com/datasets/mohankrishnathalla/sleep-health-and-daily-performance-dataset)
**Tamaño:** 100.000 registros, 10 columnas
**Última actualización:** mayo 2026

El dataset contiene mediciones de calidad y composición del sueño de 100.000 individuos pertenecientes a 15 países y 12 ocupaciones distintas. Incluye métricas objetivas (duración, porcentaje REM, porcentaje de sueño profundo) y subjetivas (calidad percibida), junto con variables demográficas (edad, género, BMI, ocupación, país).

El interés del dataset radica en que permite explorar la relación entre patrones de sueño y características demográficas/laborales en una muestra de tamaño considerable, replicando análisis frecuentes en la literatura médica sobre salud del sueño.

---

## Diccionario de datos

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `person_id` | Identificador | ID único del individuo |
| `age` | Numérica continua | Edad en años |
| `gender` | Categórica | Género autoidentificado |
| `occupation` | Categórica | Ocupación (12 categorías: Doctor, Nurse, Lawyer, Driver, Student, Software Engineer, Manager, Sales, Teacher, Homemaker, Freelancer, Retired) |
| `bmi` | Numérica continua | Índice de masa corporal (kg/m²) |
| `country` | Categórica | País de residencia (15 países) |
| `sleep_duration_hrs` | Numérica continua | Duración total del sueño en horas |
| `sleep_quality_score` | Numérica ordinal | Calidad subjetiva del sueño (1 = muy mala, 10 = excelente) |
| `rem_percentage` | Numérica continua | Porcentaje de sueño en fase REM |
| `deep_sleep_percentage` | Numérica continua | Porcentaje de sueño profundo (ondas lentas) |

### Variables derivadas 

| Columna | Descripción |
|---------|-------------|
| `deep_sleep_min` | Minutos absolutos de sueño profundo (`sleep_duration_hrs × 60 × deep_sleep_percentage/100`) |
| `rem_sleep_min` | Minutos absolutos de sueño REM |
| `age_group` | Grupo etario: 18-25, 26-40, 41-60, 60+ |
| `bmi_category` | Categoría de BMI según OMS: Bajo peso, Normal, Sobrepeso, Obesidad |
| `sleep_category` | Categoría de duración: Insuficiente (<6h), Recomendado (6-9h), Excesivo (>9h) |
| `demanda_laboral` | Nivel de demanda ocupacional: Alta, Media, Baja |
| `tipo_durmiente` | Tipología de durmiente: Eficiente, Óptimo, Privado, Ineficiente, Intermedio |

---

## 🔬 Hipótesis planteadas

1. **H1:** ¿Existe correlación positiva entre la duración del sueño y la calidad subjetiva percibida?
2. **H2:** ¿Las ocupaciones de alta demanda (Doctor, Nurse, Manager) muestran peores indicadores de sueño que las de baja demanda (Teacher, Retired, Homemaker)?
3. **H3:** ¿El BMI elevado se asocia con menor porcentaje de sueño profundo?
4. **H4:** ¿La edad correlaciona negativamente con el porcentaje de sueño REM?
5. **H5:** ¿Existen "durmientes eficientes" (poca duración pero alto % de sueño profundo y buena calidad)? ¿Qué características los distinguen?

---

## Metodología aplicada

El análisis se desarrolló en Python (Jupyter Notebook) y siguió los siguientes pasos:

1. **Carga e inspección inicial:** verificación de dimensiones, tipos de datos y estadísticos descriptivos.
2. **Limpieza de datos:** detección de nulos, duplicados, validación de rangos lógicos y coherencia interna (verificación de que REM% + Deep% ≤ 100%).
3. **Creación de variables:** minutos absolutos de sueño, grupos etarios, categorías de BMI, demanda laboral, tipología de durmientes.
4. **EDA general:**
   - 3 histogramas con KDE para variables numéricas clave
   - Detección de outliers mediante método del rango intercuartílico (IQR)
   - Matriz de correlación entre variables numéricas
5. **Análisis por hipótesis:** combinación de visualizaciones específicas (boxplots, scatterplots) con tests estadísticos (Pearson, ANOVA, Tukey HSD, t-test, Chi²).
6. **Visualizaciones adicionales:** gráfico de barras (calidad por país), torta (distribución de durmientes), líneas (evolución por edad).
7. **Conclusiones y limitaciones:** síntesis de hallazgos con discusión de las limitaciones metodológicas.

---

## Conclusiones y hallazgos relevantes

### hipótesis

| # | Hipótesis | Test estadístico | Resultado | Conclusión |
|---|-----------|------------------|-----------|------------|
| H1 | Correlación duración ↔ calidad | Pearson | r = 0.6465, p < 0.001 | **Confirmada** (correlación positiva fuerte) |
| H2 | Demanda laboral influye en sueño | ANOVA + Tukey | F = 4440.68, p < 0.001 | **Confirmada** (gradiente claro entre niveles) |
| H3 | BMI inversamente proporcional al sueño profundo | Pearson + ANOVA | r = -0.0472, F = 63.19, p < 0.001 | **Confirmada parcialmente** (dirección correcta, magnitud débil) |
| H4 | Edad disminuye % REM | Pearson | r = 0.0016, p = 0.60 | **Rechazada** (sin correlación significativa) |
| H5 | Existen durmientes eficientes | t-test + Chi² | Múltiples (p < 0.001) | **Confirmada** (perfil identificado) |

### Hallazgos 

#### 1. La duración del sueño es un predictor relevante de la calidad percibida (H1)

El análisis revela una correlación positiva fuerte entre las horas dormidas y la calidad subjetiva (r = 0.65, p < 0.001). La duración del sueño explica aproximadamente el **42% de la variabilidad** en la calidad percibida (r² ≈ 0.42), confirmando que es un factor importante pero no único: el 58% restante depende de otros factores como la composición del sueño, el estrés y los hábitos previos al descanso.

#### 2. La demanda ocupacional impacta significativamente en el sueño (H2)

Existe un gradiente claro y estadísticamente significativo entre los tres niveles de demanda laboral:

- **Baja demanda** (Teacher, Retired, Homemaker, Freelancer, Student): mejor calidad
- **Demanda media** (Software Engineer, Lawyer, Sales, Driver): intermedia
- **Alta demanda** (Doctor, Nurse, Manager): peor calidad

La diferencia entre los extremos es de **más de 1 punto** en una escala de 1 a 10, una magnitud no solo estadísticamente significativa sino también prácticamente relevante. El test post-hoc de Tukey confirma diferencias significativas en las tres comparaciones por pares (p < 0.001).

#### 3. La paradoja médica: el caso de los doctores (sub-análisis de H2)

Profundizando en el grupo de alta demanda, los doctores (N = 7.868) presentan un patrón especialmente preocupante:

- **Duración promedio: 5.93 hrs** (vs. 6.42 hrs del dataset general)
- **El 54.31% duerme menos de 6 hrs por noche** (categoría "insuficiente")
- Solo el 45.06% alcanza el rango recomendado (7-9 hrs)
- Diferencia altamente significativa respecto al resto (t = -36.07, p < 0.001)

Este es un hallazgo paradójico: los profesionales con mayor conocimiento sobre las consecuencias del mal sueño son sistemáticamente quienes peor duermen. Este patrón responde a factores estructurales del sistema sanitario (guardias rotativas, atención 24/7, residencias intensivas) y refuerza la importancia de revisar las condiciones laborales en medicina.

#### 4. El BMI tiene un impacto direccional pero débil sobre el sueño profundo (H3)

Se detectó una asociación negativa entre BMI y % de sueño profundo (r = -0.047, p < 0.001), confirmando direccionalmente lo predicho por la literatura clínica sobre apnea del sueño. Sin embargo, la **magnitud del efecto es muy débil**: el BMI explica apenas el **0.2% de la variabilidad** del sueño profundo (r² ≈ 0.002). El ANOVA entre categorías de BMI confirma diferencias significativas (F = 63.19, p < 0.001), pero con N = 100.000 incluso diferencias menores resultan estadísticamente significativas.

Este resultado ejemplifica una distinción metodológica importante: la **significancia estadística no equivale a relevancia práctica**. Con muestras grandes, casi cualquier diferencia puede detectarse como significativa, por lo que es fundamental reportar el tamaño del efecto (r, r²) además del p-valor.

#### 5. La edad no predice el porcentaje de sueño REM en este dataset (H4)

Contrario a lo esperado según la literatura clínica, **no se observa relación entre edad y % REM** (r = 0.0016, p = 0.60). Esta hipótesis fue rechazada. El resultado puede deberse a:

- Posible carácter sintético del dataset, sin replicar fielmente los patrones biológicos del envejecimiento.
- Rango etario limitado (18-69 años): los cambios más marcados en el REM ocurren en adultos mayores (70+).
- Alta variabilidad individual que enmascara el efecto poblacional.

Este resultado refuerza la importancia metodológica de **verificar empíricamente las relaciones esperadas** en lugar de asumirlas.

---

### Conclusión final

El análisis exploratorio de 100.000 registros confirma que la salud del sueño es un fenómeno multidimensional que depende de la duración, la composición fisiológica (REM y sueño profundo), las condiciones laborales y características individuales. Los hallazgos pueden sintetizarse en cinco mensajes clave:

1. **La duración importa, pero no lo es todo.** Explica el 42% de la calidad percibida; el resto responde a otros factores no capturados solo por las horas dormidas.
2. **El trabajo configura el sueño.** Existe un gradiente claro entre niveles de demanda ocupacional, con los doctores como caso paradigmático de déficit crónico estructural.
3. **La composición puede compensar la duración.** El pequeño grupo de "durmientes eficientes" logra descanso óptimo con pocas horas, sugiriendo que la calidad del sueño profundo es un recurso individual valioso.
4. **Significancia ≠ Relevancia.** El caso del BMI muestra que con muestras grandes se detectan efectos estadísticamente significativos pero prácticamente irrelevantes. Reportar tamaño del efecto es tan importante como reportar el p-valor.
5. **Verificar siempre los supuestos.** El rechazo de H4 (edad ↔ REM) demuestra la importancia de no asumir relaciones esperadas sin contrastarlas con datos reales.

### Limitaciones del estudio

- **Carácter transversal del dataset:** una sola medición por persona; no permite inferir causalidad ni efectos acumulados a largo plazo.
- **Posible origen sintético:** el rechazo de H4 y la baja magnitud de H3 sugieren que el dataset podría no replicar fielmente todos los patrones biológicos reales documentados en literatura médica. Los hallazgos deben interpretarse como ejercicio analítico, no como evidencia médica concluyente.
- **Calidad subjetiva auto-reportada:** el `sleep_quality_score` está sujeto a sesgo de percepción y deseabilidad social.
- **Sin distinción entre siestas y sueño nocturno:** el campo `sleep_duration_hrs` agrupa todo el sueño del día.
- **Variables confundentes no exploradas en profundidad:** el dataset incluye `caffeine_mg_before_bed`, `alcohol_units_before_bed`, `stress_score`, `screen_time_before_bed_mins`, `nap_duration_mins`, entre otras, que podrían enriquecer futuros análisis.

### Líneas futuras de investigación

- Analizar el impacto del consumo de cafeína, alcohol y tiempo de pantalla antes de dormir sobre la composición del sueño.
- Estudiar la relación entre el nivel de estrés (`stress_score`) y la arquitectura del sueño.
- Aplicar técnicas de clustering no supervisado (K-means, DBSCAN) para validar la tipología de durmientes propuesta en H5.
- Comparar patrones de sueño entre países para identificar diferencias culturales o sistémicas.
- Explorar la variable `nap_duration_mins` para evaluar si los durmientes eficientes compensan con siestas.
- Acceder a estudios longitudinales reales (UK Biobank, NHANES) para medir efectos acumulados a largo plazo.

---
