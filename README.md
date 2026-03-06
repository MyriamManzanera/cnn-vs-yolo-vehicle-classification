# cnn-vs-yolo-vehicle-classification
Prueba de concepto comparando una CNN custom y YOLOv8 para clasificación de vehículos con imágenes de baja resolución.
# 🚗 Visión Artificial en Logística: CNN vs YOLOv8 (PoC)

Este proyecto es una Prueba de Concepto (PoC) desarrollada como parte de la Ruta de Aprendizaje Autónomo en la asignatura Machine Learning I de UTAMED. 

El objetivo principal es demostrar la viabilidad de la visión artificial para la clasificación de vehículos de logística y tráfico, enfrentándonos a un reto técnico muy común en el Edge Computing: **trabajar con imágenes de muy baja resolución (32x32 píxeles)** extraídas del dataset CIFAR-10.

## 🎯 Objetivo del Proyecto
El experimento busca responder a una pregunta de negocio crítica: ante imágenes de baja calidad, ¿es más eficiente construir una red neuronal (CNN) a medida desde cero, o implementar un modelo avanzado de estado del arte (YOLOv8) mediante Transfer Learning?.

## 📊 Resultados Obtenidos

Para asegurar una comparativa justa, se diseñaron dos pipelines de datos distintos (en memoria RAM vía `tf.data` y exportación local) y se evaluaron las siguientes arquitecturas:

1. **CNN Custom (Red Neuronal Convolucional)**
   * **Problema:** En el modelo base se detectó un fuerte sobreajuste (*overfitting*).
   * **Solución:** Se aplicaron técnicas de regularización (Dropout al 50% y Early Stopping).
   * **Resultado:** Un modelo altamente eficiente y ligero que generaliza correctamente, alcanzando un ~82% de *accuracy* en el conjunto de Test.

2. **YOLOv8 (El Reto de la Resolución)**
   * **Experimento de Detección (`yolov8n.pt`):** Se demostró empíricamente que YOLOv8 falla como detector de objetos ante entradas de 32x32 píxeles. La falta de resolución espacial le impide calcular *bounding boxes*, generando predicciones nulas o "alucinaciones" (falsos positivos).
   * **Pivote Estratégico a Clasificación (`yolov8n-cls.pt`):** Tras diagnosticar el fallo, se adaptó el enfoque hacia la clasificación. Gracias al conocimiento previo del modelo (Transfer Learning), YOLO logró extraer patrones complejos de las imágenes de baja resolución, clasificando correctamente los vehículos.

## 💼 Recomendaciones de Negocio: ¿CNN o YOLO?

Si este prototipo se llevara a un entorno de producción real, las recomendaciones estratégicas serían:

* **Desplegar la CNN a medida SI:** El proyecto requiere inferencia en dispositivos de muy bajo coste (Edge Computing, cámaras antiguas) donde la memoria RAM y la capacidad de cómputo son extremadamente limitadas. La CNN demostró ser robusta y muy ligera.
* **Apostar por YOLOv8 SI:** La empresa planea escalar el proyecto. YOLO es la elección estratégica por su velocidad de inferencia superior y su enorme facilidad para realizar *fine-tuning* si mañana el cliente decide añadir 50 nuevas clases de vehículos al catálogo. Solo requeriría asegurar una entrada de vídeo con mayor resolución para desbloquear su potencial de detección espacial.

* ## 👩‍💻 Autora
**Myriam Manzanera Hernández**
* [Mi perfil de LinkedIn]: www.linkedin.com/in/myriammanzanera
