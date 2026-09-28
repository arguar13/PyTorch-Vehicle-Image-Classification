*Leer este documento en otros idiomas: [English](README.md)*

# Proyecto de Deep Learning — Benchmark de Clasificación Multiclase de Imágenes de Vehículos

Este proyecto desarrolla un benchmark integral de deep learning para la clasificación multiclase de imágenes de vehículos utilizando arquitecturas modernas de visión por computadora.

En lugar de centrarse en un único modelo, el proyecto evalúa el equilibrio entre rendimiento predictivo y eficiencia computacional a través de múltiples arquitecturas de deep learning, incluyendo redes neuronales convolucionales (CNNs) y Vision Transformers (ViTs).

El objetivo es clasificar imágenes de vehículos en múltiples categorías de transporte mientras se identifica la arquitectura más adecuada para escenarios de despliegue en el mundo real, como sistemas inteligentes de transporte, monitoreo de tráfico, vigilancia, automatización logística y aplicaciones de ciudades inteligentes.

---

## Acerca del Conjunto de Datos

* **Dataset:** [Vehicle Image Classification](https://www.kaggle.com/datasets/mohamedmaher5/vehicle-classification) (Kaggle, Mohamed Maher, CC0)
* **Tarea:** Clasificación Multiclase de Imágenes
* **Clases:** 7 (Auto Rickshaws, Bicicletas, Automóviles, Motocicletas, Aviones, Barcos y Trenes)
* **Total de Imágenes:** 5.590 (800 por clase, 790 en Automóviles). Se cargan 5.589: `ImageFolder` omite el único archivo `.gif`.

Las imágenes están organizadas utilizando la estructura `ImageFolder` de PyTorch.

El conjunto de datos contiene imágenes de vehículos con diferentes perspectivas, condiciones de iluminación, escalas y fondos, lo que lo hace adecuado para evaluar la robustez de los modelos y sus capacidades de generalización.

---

## Objetivos del Proyecto

El proyecto establece una canalización integral de benchmark para visión por computadora que incluye:

* Validación del conjunto de datos y control de calidad
* Análisis Exploratorio de Datos (EDA)
* Análisis de distribución de clases
* Preprocesamiento y aumento de imágenes
* División estratificada en entrenamiento / validación / test (70% / 15% / 15%)
* Balanceo de clases mediante `WeightedRandomSampler`
* Transfer Learning con modelos preentrenados en ImageNet
* Benchmark comparativo de múltiples arquitecturas
* Implementación de Early Stopping
* Evaluación de rendimiento y eficiencia
* Visualización de resultados del benchmark
* Marco de experimentación reproducible

---

## Análisis Exploratorio de Datos (EDA)

Antes del entrenamiento, el conjunto de datos se analiza para garantizar la calidad de los datos y comprender las características de cada clase.

El análisis incluye:

* Perfilado del conjunto de datos
* Visualización de la distribución de clases
* Inspección de imágenes de muestra
* Evaluación del desbalance de clases
* Análisis de la estructura de las imágenes sin procesar

Este paso ayuda a identificar posibles sesgos y orienta el diseño de las estrategias de preprocesamiento y balanceo.

---

## Preprocesamiento y Aumento de Datos

### Pipeline de Entrenamiento

El conjunto de entrenamiento se somete a diversas técnicas de aumento de datos para mejorar la capacidad de generalización del modelo:

* Redimensionamiento (256×256)
* Recorte Aleatorio (224×224)
* Volteo Horizontal Aleatorio
* Rotación Aleatoria
* Variación Aleatoria de Color (Color Jitter)
* Normalización ImageNet

### Pipeline de Validación / Test

Para una evaluación determinística (misma escala de objeto que los recortes de entrenamiento):

* Redimensionamiento (256×256)
* Recorte Central (224×224)
* Normalización ImageNet

Estas transformaciones preservan las características semánticas de los vehículos mientras mejoran la robustez frente a la variabilidad visual.

---

## Estrategia de Balanceo de Clases

El conjunto de datos está prácticamente balanceado (~800 imágenes por clase), por lo que el sampler funciona como salvaguarda para que el pipeline siga siendo correcto si la distribución de clases cambia:

* Se utiliza `WeightedRandomSampler` para realizar remuestreo dinámico
* Las clases minoritarias reciben una mayor probabilidad de muestreo
* Los datos de validación permanecen sin modificaciones para preservar una evaluación realista

Este enfoque promueve un aprendizaje equilibrado entre todas las categorías de vehículos.

---

## Arquitecturas Evaluadas

El benchmark compara seis arquitecturas de Transfer Learning preentrenadas en ImageNet:

### Modelos Basados en CNN

* ResNet18
* EfficientNet-B0
* MobileNetV3-Large
* DenseNet121
* ConvNeXt-Tiny

### Modelos Basados en Transformers

* Vision Transformer (ViT-B/16)

Cada arquitectura se adapta a la tarea de clasificación de vehículos de siete clases mediante la sustitución de la capa de clasificación original.

Las seis arquitecturas están implementadas en la fábrica de modelos. Para mantener un tiempo de entrenamiento razonable, el benchmark ejecutado corre **ResNet18** y **EfficientNet-B0**; las demás se pueden habilitar descomentándolas en la Sección 7 del notebook.

---

## Estrategia de Entrenamiento

Todos los modelos se entrenan bajo un marco experimental unificado:

* Transfer Learning
* Función de Pérdida Cross-Entropy
* Optimizador Adam
* Early Stopping sobre la pérdida de validación (paciencia = 2, se restauran los mejores pesos)
* Divisiones Estratificadas de Datos
* Selección del modelo con el conjunto de validación; el conjunto de test se usa solo para el reporte final
* Aceleración por GPU (cuando está disponible)
* Semillas Aleatorias Fijas para Reproducibilidad

Esto garantiza una comparación justa entre arquitecturas.

---

## Métricas de Evaluación

El benchmark evalúa tanto el rendimiento predictivo como la eficiencia computacional.

### Métricas Predictivas

* Accuracy
* Macro F1-Score
* Precisión
* Recall
* Reporte de Clasificación

### Métricas de Eficiencia

* Tiempo Total de Entrenamiento

Esta evaluación permite analizar tanto la calidad de los modelos como su viabilidad para despliegue en entornos reales.

---

## Análisis del Benchmark

El proyecto compara las arquitecturas según:

* Accuracy de Clasificación
* Macro F1-Score
* Tiempo de Ejecución del Entrenamiento

Los paneles de visualización proporcionan comparaciones directas entre arquitecturas y destacan los compromisos entre velocidad y rendimiento predictivo.

---

## Resultados

Resultados del benchmark ejecutado (conjunto de test estratificado de 839 imágenes; la mejor arquitectura se elige por F1 macro en validación):

| Arquitectura | F1 Macro Val | Accuracy Test | F1 Macro Test | Tiempo de Entrenamiento (s) |
|---|---|---|---|---|
| **EfficientNet-B0** | **0.976** | **0.968** | **0.968** | 573 |
| ResNet18 | 0.912 | 0.913 | 0.914 | 812 |

* **EfficientNet-B0** es el mejor modelo: ~97% de accuracy y F1 macro sobre datos no vistos, con todas las clases con un F1 de 0.95 o superior. El recall más bajo corresponde a Automóviles y Motocicletas (0.94).
* ResNet18 alcanza ~91% y su pérdida de validación oscila entre épocas, lo que sugiere que el learning rate (1e-3) es alto para hacer fine-tuning de esta red.
* Los tiempos de entrenamiento se midieron en una NVIDIA GTX 1650 (4 GB) con otras cargas de trabajo corriendo en el mismo equipo, por lo que son solo orientativos (de forma aislada, ResNet18 suele ser más rápida que EfficientNet-B0).

---

## Estructura del Proyecto

```
├── data/Vehicles/            # Dataset (no versionado, descargar de Kaggle)
├── notebooks/
│   └── vehicle_classification_benchmark.ipynb
├── requirements.txt
├── README.md
└── README_es.md
```

---

## Cómo Ejecutarlo

1. Descargar el [dataset](https://www.kaggle.com/datasets/mohamedmaher5/vehicle-classification) y extraerlo de modo que las carpetas de clases queden en `data/Vehicles/`.
2. Crear un entorno e instalar las dependencias (por defecto, PyTorch con CUDA 12.1):

   ```bash
   python -m venv .venv
   .venv\Scripts\activate        # Linux/macOS: source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. Abrir `notebooks/vehicle_classification_benchmark.ipynb` y ejecutar todas las celdas. Se recomienda usar GPU.

---

## Aplicaciones

Las posibles aplicaciones en el mundo real incluyen:

* Sistemas de Monitoreo de Tráfico
* Sistemas Inteligentes de Transporte
* Plataformas de Reconocimiento de Vehículos
* Infraestructura para Ciudades Inteligentes
* Vigilancia Automatizada
* Gestión Logística y de Flotas
* Analítica del Transporte

---

## Licencia

Proyecto con fines educativos. El conjunto de datos se utiliza exclusivamente para investigación, experimentación y fines educativos.

---

## Autor

**Armando Guarnera**

Data Scientist

Argentina