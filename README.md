# Calculadora-riesgo-esclerosis-v.1.3
# Calculadora de Esclerosis Hipocampal

**Herramienta web interactiva para estimación probabilística de esclerosis hipocampal**  
Dirección científica: **Dr. Doriam Perera** · Neurocirugía Funcional  
Estado: **Investigación en proceso — Uso exclusivamente exploratorio**

---

## ⚠️ Aviso Importante

> Esta calculadora es de **uso exclusivamente exploratorio** y forma parte de una **investigación en proceso**. **No debe ser utilizada con fines clínicos ni para tomar decisiones diagnósticas o terapéuticas en pacientes reales.** Los resultados son estimaciones estadísticas generadas a partir de una muestra pequeña (n = 40) y aún no han sido validadas externamente.

---

## 🧠 Descripción

Esta calculadora estima la **probabilidad de esclerosis hipocampal** a partir de tres mediciones morfométricas obtenidas por resonancia magnética. El usuario introduce los valores del hipocampo izquierdo y derecho para cada variable; la aplicación calcula automáticamente la diferencia porcentual entre ambos hipocampos y aplica el modelo predictivo para estimar la probabilidad.

### Variables de entrada

| Variable | Unidad | Descripción |
|---|---|---|
| Volumen hipocampal (izq. y der.) | mm³ | Volumen volumétrico de cada hipocampo |
| Grosor del giro dentado (izq. y der.) | mm | Grosor de la capa granular del giro dentado |
| Ángulo coronal de la cabeza hipocampal (izq. y der.) | grados (°) | Ángulo de inclinación coronal de la cabeza hipocampal |

La diferencia entre lados se calcula como:

```
Diferencia (%) = |mayor − menor| / mayor × 100
```

### Ecuación del modelo

```
logit(p) = −26.55 + 0.812 × DifVol + 1.128 × DifDentado + 0.230 × DifÁngulo

p = 1 / (1 + e^(−logit))
```

---

## 📊 Metodología Estadística Resumida

### 1. Diseño del estudio
- Estudio observacional de casos y controles.
- Muestra total: 46 pacientes (21 con esclerosis del hipocampo, 25 controles).
- Muestra analítica final: **n = 40** (20 casos, 20 controles), tras excluir 6 pacientes con datos incompletos en variables de ángulo/grosor.

### 2. Selección de variables
- **Análisis univariado:** Test de Mann-Whitney U (no paramétrico, adecuado para muestras pequeñas).
- **Variables seleccionadas** (las tres con mayor discriminación y datos completos):
  - Diferencia de volumen hipocampal (p < 0.0001)
  - Diferencia de grosor del giro dentado (p < 0.0001)
  - Diferencia del ángulo coronal hipocampal (p < 0.0001)
- **Variables excluidas:** Intensidad FLAIR (35% datos faltantes), grosor CA1 (alta correlación con dentado), elongación (discriminación débil, p marginal = 0.027), girificación (p = 1.0).

### 3. Modelo
- **Tipo:** Regresión logística binaria con regularización L2 (ridge, C = 5).
- **Justificación de la regularización:** Separación casi perfecta de los datos en muestra pequeña; los coeficientes MLE no convergen sin penalización (equivalente funcional al método de Firth).
- **EPV (events per variable):** 6.7 (límite inferior aceptable con regularización).

### 4. Validación interna
| Métrica | Valor |
|---|---|
| AUC aparente | 1.000 |
| AUC LOO-CV | 0.995 (IC 95%: 0.977–1.000) |
| Optimismo Bootstrap (2 000 remuestras) | 0.005 |
| Nagelkerke R² | 0.943 |
| Brier Score (LOO-CV) | 0.055 |
| Hosmer-Lemeshow p | 0.88 (buen ajuste) |

### 5. Rendimiento diagnóstico (LOO-CV)
| Métrica | Valor |
|---|---|
| Sensibilidad | 100% |
| Especificidad | 95% |
| VPP | 95.2% |
| VPN | 100% |
| Accuracy | 97.5% |
| Índice Youden | 0.95 |
| Umbral óptimo (Youden J) | p = 0.053 |

### 6. Software utilizado
- Python 3.11: `scikit-learn 1.x` (LogisticRegression), `statsmodels 0.14`, `scipy 1.x`

### 7. Limitaciones
- **Muestra pequeña (n = 40):** Riesgo de sobreajuste a pesar de validación interna favorable.
- **EPV = 6.7** está por debajo del umbral clásico de EPV ≥ 10; la regularización mitiga pero no elimina el riesgo.
- **Sin validación externa:** Los resultados deben confirmarse en cohortes independientes.
- **Datos faltantes:** Manejados con análisis de casos completos; posible sesgo de selección.

---

## 🚀 Uso

### Opción 1 — GitHub Pages (recomendado)

1. Hacer fork o clonar este repositorio.
2. En **Settings → Pages**, seleccionar la rama `main` y la carpeta raíz `/`.
3. Acceder a la URL generada por GitHub Pages (ej. `https://usuario.github.io/esclerosis-hipocampal/`).

### Opción 2 — Local

```bash
git clone https://github.com/usuario/esclerosis-hipocampal.git
cd esclerosis-hipocampal
# Abrir index.html en cualquier navegador moderno
open index.html
```

No requiere servidor ni dependencias externas. Funciona offline.

---

## 📁 Estructura del repositorio

```
esclerosis-hipocampal/
├── index.html          # Calculadora web interactiva (archivo principal)
└── README.md           # Este archivo
```

---

## 📋 Coeficientes del modelo

| Variable | β (Coeficiente) | OR por unidad | OR por 5 unidades |
|---|---|---|---|
| Intercepto (β₀) | −26.545 | — | — |
| Dif. Volumen Hipocampal (%) | 0.812 | 2.25 | 57.9 |
| Dif. Grosor Dentado (%) | 1.127 | 3.09 | 280.8 |
| Dif. Ángulo Hipocampal (%) | 0.230 | 1.26 | 3.16 |

---

## 👨‍⚕️ Autoría y contacto

**Dirección científica:** Dr. Doriam Perera · Neurocirugía Funcional  
**Estado:** Investigación en proceso — los resultados son preliminares y no han sido publicados ni validados externamente.

---

*Herramienta de uso exclusivamente exploratorio para investigación en proceso. No apta para uso clínico.*
