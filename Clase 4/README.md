# Big-Data-JDPH
# CLASE 4: ANÁLISIS DEL CÓDIGO Y MEJORAS EN EL MODELO

## ANÁLISIS GENERAL DEL CÓDIGO

Este código implementa un modelo LeNet utilizando **TensorFlow.js** para entrenar y evaluar en el conjunto de datos **MNIST** (imágenes de dígitos escritos a mano). Aunque LeNet es un modelo clásico, alcanzar un límite del **90% de precisión** es común en implementaciones básicas como esta. Analizaremos las razones detrás de este límite y cómo mejorar el desempeño.

---

## DIAGNÓSTICO: ¿POR QUÉ NO SUPERA EL 90% DE PRECISIÓN?

### ARQUITECTURA DEL MODELO
- **Capas**:  
  Las capas convolucionales y densas utilizan la activación `tanh`, que puede limitar la capacidad del modelo para capturar patrones complejos. Las activaciones como `ReLU` son más efectivas en redes profundas, ya que evitan el problema de saturación y permiten aprender representaciones jerárquicas más ricas.

### FUNCIÓN DE PÉRDIDA Y OPTIMIZADOR
- **Función de pérdida**:  
  La función `categoricalCrossentropy` es adecuada para tareas de clasificación, por lo que no es el problema principal.
- **Optimizador**:  
  El uso de **SGD (Stochastic Gradient Descent)** puede ser una limitante, ya que converge lentamente y es propenso a quedarse atrapado en mínimos locales, especialmente si no se ajusta bien la tasa de aprendizaje (*learning rate*).

### PREPROCESAMIENTO
- **Normalización**:  
  Las imágenes están normalizadas al rango [0, 1], lo cual es básico. Un preprocesamiento más avanzado, como la estandarización (restar la media y dividir por la desviación estándar), podría mejorar la precisión del modelo.

### REGULARIZACIÓN
- **Técnicas de regularización**:  
  El código no utiliza estrategias como `Dropout` o **data augmentation**, que son esenciales para mejorar la capacidad de generalización del modelo.

### DATASET
- **Variabilidad**:  
  Aunque MNIST tiene suficientes datos para este modelo, no incluye variaciones importantes (como ruido, rotaciones o deformaciones) presentes en casos del mundo real. Esto afecta la capacidad del modelo para generalizar.

### ÉPOCAS
- **Número de épocas**:  
  Solo se están entrenando 5 épocas, lo cual puede ser insuficiente para que el modelo alcance su máximo potencial.

---

## RESULTADOS DE PRUEBAS

### **TEST 01** (CÓDIGO INICIAL)
- **Entrenamiento**:  
  - Epoch 4: `loss = 0.3265`, `accuracy = 0.9000`  
- **Evaluación**:  
  - Precisión en el conjunto de prueba: **90.00%**

---

## MEJORAS IMPLEMENTADAS ANTES DE TEST 02

1. **Activación `ReLU`**:  
   Se cambió la activación de `tanh` a `ReLU` en las capas convolucionales y densas, ya que es más efectiva en redes profundas al evitar la saturación.
2. **Más filtros y unidades**:  
   Se incrementó el número de filtros en las capas convolucionales y el número de unidades en las capas densas.
3. **Optimizador `Adam`**:  
   Se sustituyó **SGD** por **Adam**, que ajusta automáticamente la tasa de aprendizaje y generalmente converge más rápido.
4. **Mayor número de épocas**:  
   Se aumentó a **20 épocas** para permitir una mejor convergencia.
5. **Dropout**:  
   Se agregó `Dropout` entre las capas densas para prevenir el sobreajuste.
6. **Data augmentation**:  
   Se aplicaron transformaciones a las imágenes (rotaciones, traslaciones e inversiones) para crear más variaciones en el conjunto de datos.

### **TEST 02**
- **Resultado obtenido**:  
  - Epoch 1: `loss = 0.3460`, `accuracy = 0.8992`

---

## MEJORAS IMPLEMENTADAS ANTES DE TEST 03

1. **L2 Regularization**:  
   Se introdujo penalización de pesos grandes para reducir el sobreajuste.
2. **Dropout**:  
   Continuó utilizándose para aumentar la robustez del modelo.
3. **Data Augmentation Avanzada**:  
   Se incrementó la diversidad del conjunto de datos mediante transformaciones adicionales.
4. **Learning Rate Scheduler**:  
   Se ajustó dinámicamente la tasa de aprendizaje según la época para mejorar la convergencia del modelo.

---

### **TEST 03**


