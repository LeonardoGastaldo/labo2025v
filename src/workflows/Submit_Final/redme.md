## Resultado Final - Modelo Seleccionado

| Métrica                 | Valor     |
|-------------------------|-----------|
| **Mejor ganancia Kaggle** | 4160    |
| **Corte óptimo**          | 900     |
| **Workflow elegido**      | WF63156 |
| **Familia más estable**   | X900    |
| **CV de la familia**      | 0.0448  |
| **Media familia X900**    | 4032.62 |

---

## Cómo reproducir

### 1. Requisitos
- Google Cloud con Runtime R
- Cuenta de Kaggle configurada

### 2. Copiar en `datasets/` los archivos:
- `Indice-FACPCE.xlsx`
- `gerencial_competencia_2025.csv.gz`

### 3. Ejecutar script
[Notebook en GitHub](https://github.com/LeonardoGastaldo/labo2025v/blob/main/src/workflows/Submit_Final/610_WorkFlow_FINAL_530797_WF63156_gerencial.ipynb)
src/workflows/Submit_Final/610_WorkFlow_FINAL_530797_WF63156_gerencial.ipynb

### 4. Estructura de carpetas esperada
```
/content/buckets/b1/
├── datasets/
│   └── gerencial_competencia_2025.csv.gz
│   └── Indice-FACPCE.xlsx
├── kaggle/
│   └── kaggle.json
└── exp/
    └── WF63156/
        ├── HT.RDATA
        ├── BO_log.txt
        ├── PARAM.yml
        └── kaggle/
            └── KA63156_*.csv
```

---

## Metodología

### 1. Catastrophe Analysis
- **Mes afectado:** 202006 (COVID-19)
- **Método:** `"MachineLearning"` que asigna `NA` a datos anómalos.

### 2. DR – Data Drifting
- Ajuste por inflación usando el archivo `Indice-FACPCE.xlsx`.

### 3. Feature Engineering histórico

#### FEATURE 1: RATIOS & RENTABILIDAD (`ratio_profitability_score`)
- Rentabilidad anual
- Variaciones (`delta1`, `delta2`, `lag2`)
- Ingresos relativos (`mpayroll_sobre_edad`)
- Margen activos/pasivos

* Alto → cliente sano  
* Bajo → potencial estrés financiero

#### FEATURE 2: SALDOS & VARIACIONES (`balance_trend_score`)
- Saldo de cuentas
- Variaciones de saldo
- Préstamos y tarjetas

* Alto → crecimiento  
* Bajo → señales de salida

#### FEATURE 3: ACTIVIDAD & USO OPERATIVO (`activity_behavior_score`)
- Transacciones (`ctrx_quarter`)
- Uso de Visa (`Visa_msaldototal`)
- Comisiones (`lag1`, `lag2`)
- Homebanking

* Alto → cliente activo  
* Bajo → cliente desconectado

---

## Entrenamiento y Optimización

### Training Strategy
- Estrategia de pesos por año
- Optimización Bayesiana con `mlrMBO`

### Optimización
- **Método:** Bayesian Optimization (`mlrMBO`)
- **Iteraciones:** 60
- **Métrica:** `auc`
- **max_bin:** 31

### Hiperparámetros optimizados

```json
{
  "num_leaves": 2583,
  "min_data_in_leaf": 3,
  "lambda_l2": 1.6265921337433,
  "learning_rate": 0.0373501030456391,
  "num_iterations": 3625,
  "feature_fraction": 0.089941639526016
}
```
