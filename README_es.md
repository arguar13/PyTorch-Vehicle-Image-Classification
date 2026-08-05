*Leer este documento en otros idiomas: [English](README.md)*

# Proyecto de Deep Learning — Benchmark de Clasificación Multiclase de Imágenes de Vehículos

Este proyecto desarrolla un benchmark integral de deep learning para la clasificación multiclase de imágenes de vehículos utilizando arquitecturas modernas de visión por computadora.

En lugar de centrarse en un único modelo, el proyecto evalúa el equilibrio entre rendimiento predictivo y eficiencia computacional a través de múltiples arquitecturas de deep learning, incluyendo redes neuronales convolucionales (CNNs) y Vision Transformers (ViTs).

El objetivo es clasificar imágenes de vehículos en múltiples categorías de transporte mientras se identifica la arquitectura más adecuada para escenarios de despliegue en el mundo real, como sistemas inteligentes de transporte, monitoreo de tráfico, vigilancia, automatización logística y aplicaciones de ciudades inteligentes.

---

## Acerca del Conjunto de Datos

* **Dataset:** Vehicle Image Classification Dataset
* **Tarea:** Clasificación Multiclase de Imágenes
* **Clases:** 7 (Auto Rickshaws, Bicicletas, Automóviles, Motocicletas, Aviones, Barcos y Trenes)
* **Total de Imágenes:** Aproximadamente 5.600

Las imágenes están organizadas utilizando la estructura `ImageFolder` de PyTorch.

El conjunto de datos contiene imágenes de vehículos con diferentes perspectivas, condiciones de iluminación, escalas y fondos, lo que lo hace adecuado para evaluar la robustez de los modelos y sus capacidades de generalización.

---

## Objetivos del Proyecto

El proyecto establece una canalización integral de benchmark para visión por computadora que incluye:

* Validación del conjunto de datos y control de calidad
* Análisis Exploratorio de Datos (EDA)
* Análisis de distribución de clases
* Preprocesamiento y aumento de imágenes
* División estratificada entre entrenamiento y validación
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

### Pipeline de Validación

Para una evaluación determinística:

* Redimensionamiento (224×224)
* Normalización ImageNet

Estas transformaciones preservan las características semánticas de los vehículos mientras mejoran la robustez frente a la variabilidad visual.

---

## Estrategia de Balanceo de Clases

Para mitigar el desbalance de clases durante el entrenamiento:

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

---

## Estrategia de Entrenamiento

Todos los modelos se entrenan bajo un marco experimental unificado:

* Transfer Learning
* Función de Pérdida Cross-Entropy
* Optimizador Adam
* Early Stopping
* Divisiones Estratificadas de Datos
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