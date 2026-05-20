# Cancer Prediction Dashboard

Dashboard interactivo del caso de uso *"Predicción de Diagnóstico de Cáncer"*
(UAX, Ingeniería Matemática, IA · 2025-2026).

**Inferencia en vivo en el navegador.** Cero servidor. Cero build. Todo el
modelo de Regresión Logística está embebido en JSON y se ejecuta con
JavaScript puro — los resultados son **bit-idénticos** a los de sklearn
(verificado a machine epsilon en el script `src/export_web.py`).

## Abrir en local

Opción 1 — doble clic en `index.html`. Funciona offline excepto los CDNs.

Opción 2 — servidor estático (recomendado para evitar restricciones de
`fetch` en `file://`):

```bash
python -m http.server 8000
# Abrir http://localhost:8000
```

## Verificación numérica

Al cargar la página, abre DevTools (F12) → pestaña Console. Debe aparecer:

```
✓ JS inference verified: match sklearn within 0.001 (diff=X.XXe-XX, paciente id=NN)
```

Si no aparece, hay bug en el orden del vector de features o en el
preprocesado — revisar `predictLogReg()` en `index.html`.

## Deploy en GitHub Pages

Este repositorio ya está inicializado y con un commit hecho. Para publicarlo:

1. Crear un repo **público y vacío** en GitHub (sin README ni .gitignore),
   por ejemplo `prediccion-cancer-uax`.
2. Desde esta carpeta:
   ```bash
   git remote add origin https://github.com/<usuario>/prediccion-cancer-uax.git
   git push -u origin main
   ```
3. En el repo: Settings → Pages → Source: `main` branch / `/ (root)` → Save.
4. URL: `https://<usuario>.github.io/prediccion-cancer-uax/`
   (tarda 1-2 min en estar disponible la primera vez).

El fichero `.nojekyll` desactiva el procesador Jekyll de GitHub Pages
(necesario para servir cualquier path que empiece por `_`).

## Estructura

```
.
├── index.html                    # todo en uno: HTML + CSS + JS
├── slides.html                   # diapositivas del caso de uso
├── .nojekyll                     # GitHub Pages: skip Jekyll
├── README.md                     # este fichero
└── data/
    ├── logreg_model.json         # pesos LogReg + scaler + schemas + thresholds
    ├── test_predictions.json     # 7501 pacientes × 5 probas (837 KB)
    ├── roc_curves.json           # ROC + PR de los 5 modelos
    ├── coefficients_table.json   # 39 coefs ordenados
    └── test_features_sample.json # 50 pacientes reales para el simulador
```

## Stack

HTML5 + Tailwind CSS (CDN) + Chart.js 4 (CDN) + Alpine.js 3 (CDN).
Sin build step. Sin npm. Sin framework JS. Sin servidor.

## Métricas que muestra el dashboard (test, n=7.501)

| Modelo | t* | F1 | AUC-ROC |
|---|---|---|---|
| LogReg | 0,63 | 0,577 | 0,846 |
| XGBoost | 0,60 | 0,578 | 0,847 |
| RandomForest | 0,55 | 0,571 | 0,843 |
| LightGBM | 0,58 | 0,566 | 0,840 |
| MLP | 0,60 | 0,552 | 0,821 |

Modelo recomendado: **LogReg en punto operativo clínico** (t=0,38) →
sensibilidad 86,5 %, 2,74 biopsias por cáncer detectado.
