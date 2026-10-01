
### Resultados observados en esta ejecución

**Hardware:** Windows 10, Python 3.9.25, 6 CPU físicas /
12 lógicas, 7.27 GB de RAM (0.52 GB disponibles al inicio).
Gradient boosting en sklearn: `HistGradientBoostingClassifier`.

> **Limitación de esta ejecución:** los modelos de PySpark ['GradientBoosting'] no pudieron completarse en el equipo disponible (~7 GB de RAM; la JVM cayó tras 158 min). Las comparaciones entre entornos se realizan sobre los 5 modelos presentes en ambos.

**1. ¿Qué entorno fue más rápido?** Suma de entrenamiento + búsqueda de hiperparámetros: sklearn = 11,836.5 s,
PySpark = 1,331.8 s. Carga: sklearn = 255.4 s, PySpark = 39.7 s.
Preprocesamiento: sklearn = 19.5 s, PySpark = 42.6 s. Más rápido por modelo:
  - LogisticRegression: sklearn
  - DecisionTree: PySpark
  - RandomForest: PySpark
  - LinearSVC: sklearn
  - NaiveBayes: sklearn

**2. ¿Cuál fue más preciso?** Mayor ROC-AUC: GradientBoosting (sklearn) = 0.7288.
Mayor F1: LogisticRegression (pyspark) = 0.3352.

**3–4. Diferencias de AUC (DeLong) significativas tras Holm y relevantes en la práctica:**
- **A. Entre entornos** — significativas tras Holm: 4 de 5; además relevantes (|ΔAUC| ≥ 0.005): 1
  - sklearn:DecisionTree vs spark:DecisionTree: ΔAUC=+0.0177
- **B. Dentro de sklearn** — significativas tras Holm: 14 de 15; además relevantes (|ΔAUC| ≥ 0.005): 12
  - sklearn:LogisticRegression vs sklearn:DecisionTree: ΔAUC=+0.0115
  - sklearn:LogisticRegression vs sklearn:LinearSVC: ΔAUC=+0.0150
  - sklearn:LogisticRegression vs sklearn:NaiveBayes: ΔAUC=+0.0217
  - sklearn:DecisionTree vs sklearn:RandomForest: ΔAUC=-0.0112
  - sklearn:DecisionTree vs sklearn:GradientBoosting: ΔAUC=-0.0163
  - sklearn:DecisionTree vs sklearn:NaiveBayes: ΔAUC=+0.0102
  - sklearn:RandomForest vs sklearn:GradientBoosting: ΔAUC=-0.0050
  - sklearn:RandomForest vs sklearn:LinearSVC: ΔAUC=+0.0148
  - sklearn:RandomForest vs sklearn:NaiveBayes: ΔAUC=+0.0214
  - sklearn:GradientBoosting vs sklearn:LinearSVC: ΔAUC=+0.0198
  - sklearn:GradientBoosting vs sklearn:NaiveBayes: ΔAUC=+0.0264
  - sklearn:LinearSVC vs sklearn:NaiveBayes: ΔAUC=+0.0067
- **C. Dentro de PySpark** — significativas tras Holm: 10 de 10; además relevantes (|ΔAUC| ≥ 0.005): 10
  - spark:LogisticRegression vs spark:DecisionTree: ΔAUC=+0.0337
  - spark:LogisticRegression vs spark:RandomForest: ΔAUC=+0.0083
  - spark:LogisticRegression vs spark:LinearSVC: ΔAUC=+0.0163
  - spark:LogisticRegression vs spark:NaiveBayes: ΔAUC=+0.0262
  - spark:DecisionTree vs spark:RandomForest: ΔAUC=-0.0254
  - spark:DecisionTree vs spark:LinearSVC: ΔAUC=-0.0175
  - spark:DecisionTree vs spark:NaiveBayes: ΔAUC=-0.0075
  - spark:RandomForest vs spark:LinearSVC: ΔAUC=+0.0080
  - spark:RandomForest vs spark:NaiveBayes: ΔAUC=+0.0179
  - spark:LinearSVC vs spark:NaiveBayes: ΔAUC=+0.0099

**5. Concordancia de las tres pruebas (DeLong, McNemar, bootstrap):** {'coinciden': 6, 'coinciden parcialmente': 1}.
DeLong y bootstrap-ΔAUC con la misma conclusión en 7 de 7 comparaciones.

**6. Volumen:** Con este hardware PySpark nunca superó a scikit-learn en el rango probado (hasta 1,808,560 filas).

**7. LIME:** mejor modelo con probabilidades — sklearn: GradientBoosting, PySpark: LogisticRegression. Tiempo medio por explicación —
sklearn: 0.2 s (5000 muestras),
PySpark: 13.3 s (500 muestras). Variables más influyentes:
  - sklearn / GradientBoosting / id 58191133: 0.00 < inq_last_12m_missing <= 1.00; 9.49 < int_rate <= 12.62; fico_range_low > 715.00
  - sklearn / GradientBoosting / id 8608795: int_rate > 15.61; 0.00 < inq_last_12m_missing <= 1.00; acc_open_past_24mths <= 2.00
  - pyspark / LogisticRegression / id 85138321: int_rate > 15.61; sub_grade=D2; inq_last_12m_missing <= 0.00
  - pyspark / LogisticRegression / id 42905253: sub_grade=C5; 0.00 < inq_last_12m_missing <= 1.00; 0.00 < all_util_missing <= 1.00
