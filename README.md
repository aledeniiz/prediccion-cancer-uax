# Predicción de Diagnóstico de Cáncer

Caso de uso de Inteligencia Artificial aplicado al **cribado clínico**: un
modelo que estima, a partir de datos clínicos de un paciente, la probabilidad
de un diagnóstico de cáncer, para ayudar a decidir a quién conviene hacer
pruebas adicionales.

Trabajo de **Ingeniería Matemática · Universidad Alfonso X el Sabio · 2025/26**.

## 🔗 Web del proyecto

**Dashboard interactivo:** https://aledeniiz.github.io/prediccion-cancer-uax/

No requiere instalar nada: se abre en el navegador desde cualquier dispositivo.

## El problema

Un modelo de cribado tiene que mantener un **equilibrio de costes**:

- Si **detecta de más**, se hacen pruebas y biopsias costosas a personas
  sanas — gasto sanitario en vano.
- Si **detecta de menos**, hay cánceres que pasan desapercibidos y avanzan;
  el tratamiento en fase avanzada es mucho más caro y de peor pronóstico.

El objetivo no es solo "acertar", sino elegir el modelo y el punto de
decisión que minimicen el coste total de ambos errores.

## Qué puedes ver en el dashboard

- **Comparación de modelos** — regresión logística, Random Forest, XGBoost,
  LightGBM y una red neuronal, evaluados sobre un conjunto de test con
  intervalos de confianza.
- **Simulador de pacientes** — la predicción del modelo se calcula en vivo en
  el navegador, sin servidor.
- **Interpretabilidad** — el peso de cada factor de riesgo en la decisión del
  modelo (marcadores genéticos, hábitos, etc.).
- **Punto de operación clínico** — cómo cambia el resultado al ajustar el
  umbral de decisión.

## Conclusión del trabajo

Se recomienda la **Regresión Logística** con un **umbral de decisión de 0,38**:

| Métrica | Valor |
|---|---|
| Sensibilidad | 86,5 % (1.252 de 1.447 cánceres detectados) |
| Modelo | 39 coeficientes interpretables, auditable, sin caja negra |
| Rendimiento | estadísticamente equivalente a XGBoost, pero más simple |

Rinde igual que los modelos más complejos pero es transparente: un comité
clínico puede leer y revisar cada coeficiente. Al estar validado sobre datos
sintéticos, un uso real exigiría reentrenar con población verdadera y
monitorizar la deriva del modelo.

## Cómo funciona la web

Una sola página HTML, sin servidor ni build. El modelo de regresión logística
(pesos, escalado y umbrales) está embebido en ficheros JSON y la inferencia se
ejecuta con JavaScript puro — los resultados son idénticos a los de la
librería scikit-learn usada en el entrenamiento.

Stack: HTML5 + Tailwind CSS + Chart.js + Alpine.js (todo por CDN).

## Estructura

```
.
├── index.html      # dashboard interactivo (HTML + CSS + JS en un archivo)
└── data/           # modelo y resultados embebidos (JSON)
```

## Autor

Alejandro Déniz Solana — Grado en Ingeniería Matemática, UAX.
