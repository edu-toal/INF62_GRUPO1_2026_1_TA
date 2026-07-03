# Adaptación de dominio para detección de enfermedades en hojas de papa

Este repositorio contiene la implementación experimental de estrategias de adaptación de dominio para la clasificación de enfermedades foliares en cultivos de papa. El objetivo es evaluar la caída de desempeño de un modelo entrenado con imágenes controladas y su mejora al adaptarlo a imágenes reales de campo.

El problema se aborda mediante transferencia de aprendizaje con redes convolucionales preentrenadas en ImageNet. La arquitectura principal es EfficientNetB0, mientras que VGG16 se conserva como experimento preliminar comparativo.

## Objetivo

Evaluar estrategias de adaptación de dominio para mejorar la generalización de un clasificador de hojas de papa entre un dominio fuente controlado y un dominio objetivo de campo.

Las clases evaluadas son:

- `healthy`
- `early_blight`
- `late_blight`

## Datasets

Se emplean dos conjuntos de imágenes:

| Dataset | Rol | Descripción |
|---|---|---|
| PLD | Dominio fuente | Imágenes de hojas de papa en condiciones controladas |
| Dataset Tanzano | Dominio objetivo | Imágenes de hojas de papa capturadas en campo |

Las etiquetas originales de ambos datasets se homologan a tres clases comunes: `healthy`, `early_blight` y `late_blight`.

## Estrategias evaluadas

| Modelo | Descripción |
|---|---|
| MB | Modelo base entrenado solo con PLD |
| MA | Modelo con aumento de datos orientado a variaciones de campo |
| MFT | Modelo MA adaptado con K-shot del dominio objetivo |

La estrategia MFT utiliza 20 imágenes por clase del dominio objetivo para el ajuste fino.

## Arquitectura principal

El modelo final utiliza EfficientNetB0 preentrenada en ImageNet como extractor de características. Sobre esta base se añade un cabezal de clasificación compuesto por:

- `GlobalAveragePooling2D`
- `Dropout`
- Capa densa de 128 neuronas con activación ReLU
- Capa de salida Softmax de 3 clases

EfficientNetB0 fue seleccionada por su equilibrio entre desempeño y costo computacional. En pruebas preliminares, VGG16 obtuvo un desempeño cercano, pero EfficientNetB0 alcanzó mejores métricas finales con una arquitectura más compacta.

## Resultados principales

La evaluación final se realizó sobre el dominio objetivo Tanzano.

| Modelo | Accuracy | Precision macro | Recall macro | F1-score macro |
|---|---:|---:|---:|---:|
| MB | 0.6389 | 0.7967 | 0.6389 | 0.6182 |
| MA | 0.7711 | 0.8522 | 0.7711 | 0.7510 |
| MFT | 0.9300 | 0.9330 | 0.9300 | 0.9297 |

Los resultados muestran que el aumento de datos mejora el desempeño frente al modelo base. Sin embargo, el ajuste fino K-shot obtiene la mayor mejora, lo que indica que una pequeña muestra balanceada del dominio objetivo puede reducir de forma significativa la brecha entre imágenes controladas y de campo.

## Estructura del repositorio

```text
.
├── FINAL_modelo_adaptacion_dominio_papa.ipynb
├── PREELIMINAR_VGG16_modelo_adaptacion_dominio_papa.ipynb
├── README.md
└── outputs/
    ├── models/
    ├── figures/
    ├── tables/
    └── logs/
