## Análisis Exploratorio de Datos (EDA) ##

Repositorio del TP, centrado en un análisis exploratorio de datos sobre patrones de sueño y rendimiento diario.

---

###  Objetivo  ###

Desarrollar un Análisis Exploratorio de Datos (EDA) sobre un dataset real, aplicando técnicas de limpieza, transformación y visualización para responder un conjunto de hipótesis previamente definidas y extraer conclusiones relevantes sobre los patrones de sueño de la población.

---

### Contexto del dataset ###

**Nombre:** Sleep Health & Daily Performance Dataset
**Fuente:** [Kaggle](https://www.kaggle.com/datasets/mohankrishnathalla/sleep-health-and-daily-performance-dataset)
**Tamaño:** 100.000 registros, 10 columnas
**Última actualización:** mayo 2026

El dataset contiene mediciones de calidad y composición del sueño de 100.000 individuos pertenecientes a 15 países y 12 ocupaciones distintas. Incluye métricas objetivas (duración, porcentaje REM, porcentaje de sueño profundo) y subjetivas (calidad percibida), junto con variables demográficas (edad, género, BMI, ocupación, país).

El interés del dataset radica en que permite explorar la relación entre patrones de sueño y características demográficas/laborales en una muestra de tamaño considerable, replicando análisis frecuentes en la literatura médica sobre salud del sueño.

---

### Diccionario de datos ###

### Columnas originales

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

---

### Metodología aplicada ###

El análisis se desarrolló en Python (Jupyter Notebook) y siguió los siguientes pasos:

1. **Carga e inspección inicial:** verificación de dimensiones, tipos de datos y estadísticos descriptivos.
2. **Limpieza de datos:** detección de nulos, duplicados, validación de rangos lógicos y coherencia interna (verificación de que REM% + Deep% ≤ 100%).
3. **Feature engineering:** creación de variables derivadas (minutos absolutos de sueño, grupos etarios, categorías de BMI, demanda laboral, tipología de durmientes).
4. **EDA general:**
   - 3 histogramas con KDE para variables numéricas clave
   - Detección de outliers mediante método del rango intercuartílico (IQR)
   - Matriz de correlación entre variables numéricas
5. **Análisis por hipótesis:** combinación de visualizaciones específicas (boxplots, scatterplots) con tests estadísticos (Pearson, ANOVA, Tukey HSD, t-test, Chi²).
6. **Visualizaciones adicionales:** gráfico de barras (calidad por país), torta (distribución de durmientes), líneas (evolución por edad).
7. **Conclusiones y limitaciones:** síntesis de hallazgos con discusión de las limitaciones metodológicas.

---

## 🔬 Hipótesis planteadas

1. **H1:** ¿Existe correlación positiva entre la duración del sueño y la calidad subjetiva percibida?
2. **H2:** ¿Las ocupaciones de alta demanda (Doctor, Nurse, Manager) muestran peores indicadores de sueño que las de baja demanda (Teacher, Retired, Homemaker)?
3. **H3:** ¿El BMI elevado se asocia con menor porcentaje de sueño profundo?
4. **H4:** ¿La edad correlaciona negativamente con el porcentaje de sueño REM?
5. **H5:** ¿Existen "durmientes eficientes" (poca duración pero alto % de sueño profundo y buena calidad)? ¿Qué características los distinguen?

---

## 🎯 Conclusiones y hallazgos relevantes

### Resumen por hipótesis

| # | Hipótesis | Test estadístico | Resultado | Conclusión |
|---|-----------|------------------|-----------|------------|
| H1 | Correlación duración ↔ calidad | Pearson | r = 0.6465, p < 0.001 | ✅ **Confirmada** (correlación positiva fuerte) |
| H2 | Demanda laboral influye en sueño | ANOVA + Tukey | F = 4440.68, p < 0.001 | ✅ **Confirmada** (gradiente claro entre niveles) |
| H3 | BMI inversamente proporcional al sueño profundo | Pearson + ANOVA | _Pendiente_ | _Pendiente_ |
| H4 | Edad disminuye % REM | Pearson | r = 0.0016, p = 0.60 | ❌ **Rechazada** (sin correlación significativa) |
| H5 | Existen durmientes eficientes | t-test + Chi² | Múltiples (p < 0.001) | ✅ **Confirmada** (perfil identificado) |

---

### Hallazgos destacados

#### 1. La duración del sueño es un predictor relevante de la calidad percibida (H1)

El análisis revela una correlación positiva fuerte entre las horas dormidas y la calidad subjetiva (r = 0.65, p < 0.001). La duración del sueño explica aproximadamente el **42% de la variabilidad** en la calidad percibida, confirmando que es un factor importante pero no único: el 58% restante depende de otros factores como la composición del sueño, el estrés y los hábitos previos al descanso.

#### 2. La demanda ocupacional impacta significativamente en el sueño (H2)

Existe un gradiente claro y estadísticamente significativo entre los tres niveles de demanda laboral:

- **Baja demanda** (Teacher, Retired, Homemaker, Freelancer, Student): mejor calidad
- **Demanda media** (Software Engineer, Lawyer, Sales, Driver): intermedia
- **Alta demanda** (Doctor, Nurse, Manager): peor calidad

La diferencia entre los extremos es de **más de 1 punto** en una escala de 1 a 10, una magnitud no solo estadísticamente significativa sino también prácticamente relevante.

#### 3. La paradoja médica: el caso de los doctores (sub-análisis de H2)

Profundizando en el grupo de alta demanda, los doctores (N = 7.868) presentan un patrón especialmente preocupante:

- **Duración promedio: 5.93 hrs** (vs. 6.42 hrs del dataset general)
- **El 54.31% duerme menos de 6 hrs por noche** (categoría "insuficiente")
- Solo el 45.06% alcanza el rango recomendado (7-9 hrs)
- Diferencia altamente significativa respecto al resto (t = -36.07, p < 0.001)

Este es un hallazgo paradójico: los profesionales con mayor conocimiento sobre las consecuencias del mal sueño son sistemáticamente quienes peor duermen. Este patrón responde a factores estructurales del sistema sanitario (guardias rotativas, atención 24/7, residencias intensivas) y refuerza la importancia de revisar las condiciones laborales en medicina.

#### 4. La edad no predice el porcentaje de sueño REM en este dataset (H4)

Contrario a lo esperado según la literatura clínica, **no se observa relación entre edad y % REM** (r = 0.0016, p = 0.60). Esta hipótesis fue rechazada. El resultado puede deberse a:

- Posible carácter sintético del dataset, sin replicar fielmente los patrones biológicos del envejecimiento.
- Rango etario limitado (18-69 años): los cambios más marcados en el REM ocurren en adultos mayores (70+).
- Alta variabilidad individual que enmascara el efecto poblacional.

Este resultado refuerza la importancia metodológica de **verificar empíricamente las relaciones esperadas** en lugar de asumirlas.

#### 5. Existen "durmientes eficientes": el hallazgo más original (H5)

El análisis identificó un fenotipo característico: **287 personas (0.29% del dataset)** que con poca duración de sueño logran composiciones altamente reparadoras y reportan buena calidad subjetiva.

**Características distintivas del durmiente eficiente:**

- **Edad promedio: 29.2 años** (vs. 40.3 en durmientes privados, p < 0.001) → significativamente más jóvenes
- **Minutos absolutos de sueño profundo: 79.65 min** vs. 46.44 en privados (p < 0.001) → **71% más de sueño profundo absoluto** durmiendo prácticamente la misma cantidad de horas (5.29 vs 5.03)
- **Ocupaciones predominantes:** Estudiantes (24%), Software Engineers (20%), Sales (13%) — perfiles con horarios flexibles
- **Distribución por género equilibrada:** 50.9% mujeres, 46.0% hombres, 3.1% otros

Este hallazgo desafía la idea simplista de que "dormir más es mejor" y refuerza que la **composición** del sueño puede ser tanto o más importante que la **duración** total.

---

### Conclusión final

El análisis exploratorio de 100.000 registros confirma que la salud del sueño es un fenómeno multidimensional: depende de la duración, la composición fisiológica (REM y sueño profundo), las condiciones laborales y características individuales como la edad. Los hallazgos principales pueden sintetizarse en cuatro mensajes clave:

1. **La duración importa, pero no lo es todo.** Explica el 42% de la calidad percibida; el resto depende de otros factores.
2. **El trabajo configura el sueño.** Existe un gradiente claro entre niveles de demanda ocupacional, con los doctores como caso paradigmático de déficit crónico.
3. **La composición puede compensar la duración.** Un pequeño grupo de "durmientes eficientes" logra descanso óptimo con pocas horas, sugiriendo que la calidad del sueño profundo es un recurso individual valioso.
4. **Verificar siempre los supuestos.** El rechazo de H4 (edad ↔ REM) demuestra la importancia de no asumir relaciones esperadas sin contrastarlas con datos.

### Limitaciones del estudio

- **Carácter transversal del dataset:** una sola medición por persona; no permite inferir causalidad ni efectos acumulados a largo plazo.
- **Posible origen sintético:** el rechazo de H4 sugiere que el dataset podría no replicar fielmente todos los patrones biológicos reales. Los hallazgos deben interpretarse como ejercicio analítico, no como evidencia médica concluyente.
- **Calidad subjetiva auto-reportada:** el `sleep_quality_score` está sujeto a sesgo de percepción.
- **Sin distinción siestas/sueño nocturno:** el campo de duración agrupa todo el sueño del día.
- **Variables potencialmente confundentes no analizadas en profundidad:** el dataset incluye `caffeine_mg_before_bed`, `alcohol_units_before_bed`, `stress_score`, `screen_time_before_bed_mins`, entre otras, que podrían enriquecer futuros análisis.

### Líneas futuras de investigación

- Analizar el impacto del consumo de cafeína, alcohol y tiempo de pantalla antes de dormir sobre la composición del sueño.
- Estudiar la relación entre el nivel de estrés (`stress_score`) y la arquitectura del sueño.
- Aplicar técnicas de clustering no supervisado (K-means, DBSCAN) para validar la tipología de durmientes propuesta en H5.
- Comparar patrones de sueño entre países para identificar diferencias culturales o sistémicas.
- Acceder a estudios longitudinales reales (UK Biobank, NHANES) para medir efectos acumulados.

---

## 🎯 Conclusiones y hallazgos relevantes

_(Esta sección debe completarse tras ejecutar el notebook con los datos reales)_

### Resumen por hipótesis

| # | Hipótesis | Resultado | Significancia |
|---|-----------|-----------|---------------|
| H1 | Correlación duración ↔ calidad | _Pendiente_ | _Pendiente_ |
| H2 | Demanda laboral influye en sueño | _Pendiente_ | _Pendiente_ |
| H3 | BMI inversamente proporcional al sueño profundo | _Pendiente_ | _Pendiente_ |
| H4 | Edad disminuye % REM | _Pendiente_ | _Pendiente_ |
| H5 | Existen durmientes eficientes | _Pendiente_ | _Pendiente_ |

### Hallazgos destacados

_(Completar al finalizar el análisis. Estructura sugerida:)_

1. **Sobre la duración del sueño:** ___
2. **Sobre el impacto ocupacional:** ___
3. **Sobre el efecto del BMI:** ___
4. **Sobre el envejecimiento del sueño:** ___
5. **Hallazgo más interesante (H5):** ___

### Limitaciones

- **Dataset transversal:** una sola medición por persona; no permite inferir causalidad ni efectos acumulados a largo plazo.
- **Variables ausentes:** no se incluyen datos sobre dieta, ejercicio, consumo de cafeína/alcohol ni medicación.
- **Calidad subjetiva:** el `sleep_quality_score` es autorreportado, sujeto a sesgo de percepción.
- **Sin distinción entre siestas y sueño nocturno:** el campo de duración agrupa el sueño total.
- **Origen del dataset:** parte de los registros podrían ser sintéticos. Los hallazgos se interpretan como ejercicio analítico, no como evidencia médica concluyente.

### Líneas futuras

- Integrar información nutricional para evaluar el impacto de la dieta sobre la composición del sueño.
- Usar estudios longitudinales (UK Biobank, NHANES) para medir efectos a largo plazo.
- Aplicar técnicas de clustering no supervisado (K-means, DBSCAN) para validar la tipología de durmientes.


---

## ▶️ Cómo reproducir el análisis

1. Clonar este repositorio:
   ```bash
   git clone <URL-del-repo>
   cd <nombre-del-repo>
   ```

2. Instalar dependencias:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels jupyter
   ```

3. Descargar el dataset desde [Kaggle](https://www.kaggle.com/datasets/mohankrishnathalla/sleep-health-and-daily-performance-dataset) y colocarlo en la raíz del proyecto como `sleep_health_dataset.csv`.

4. Ejecutar el notebook:
   ```bash
   jupyter notebook TP1_EDA_Sleep_Health.ipynb
   ```

