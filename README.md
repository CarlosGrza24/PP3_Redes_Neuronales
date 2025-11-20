# Clasificación de Dígitos con Redes Neuronales Convolucionales

## Descripción del Proyecto

Este proyecto desarrolla un sistema de clasificación de imágenes capaz de reconocer dígitos escritos a mano (0–9) mediante **Redes Neuronales Convolucionales (CNN)**.  
Se entrenaron múltiples arquitecturas, se comparó su desempeño y se seleccionó el modelo óptimo para:

- Evaluarlo en un conjunto de prueba independiente.  
- Implementarlo en **tiempo real** mediante webcam y OpenCV.  
- Extenderlo a un esquema de **detección múltiple** (OCR).


##  1. Importación y Preparación de Imágenes

Las imágenes se cargaron desde Google Drive con una estructura jerárquica.

Preprocesamiento aplicado:
- Conversión a **escala de grises**.  
- Redimensionamiento a **280 × 280 px**.  
- Normalización al rango **[0, 1]**.  
- Separación automática del conjunto de entrenamiento en **80% train / 20% validación**.

---

## 2. Arquitectura Base de CNN

La primera arquitectura (baseline) incluyó:
- 3 bloques convolucionales con filtros **32, 64 y 128**.  
- `MaxPooling2D` después de cada bloque.  
- `Flatten()` → vectorización.  
- `Dense(128, ReLU)` + `Dropout(0.5)`.  
- Capa final `Dense(10, softmax)`.

Se entrenó durante **7 épocas** con **Adam** y **EarlyStopping**.

---

## 3. Ajuste de Hiperparámetros (6 modelos en total)

Se desarrollaron seis arquitecturas distintas:

### Modelo 1 – Baseline CNN  
3 bloques conv.

### Modelo 2 – CNN simplificada  
Menos filtros, 2 bloques conv.

### Modelo 3 – CNN profunda (modelo ganador)  
4 bloques conv con filtros: **32 → 64 → 128 → 256**.

### Modelo 4 – CNN con Batch Normalization  
BN para estabilizar el gradiente.

### Modelo 5 – CNN con filtros 5×5  
Filtros más grandes → captura de patrones más amplios.

### Modelo 6 – CNN con GlobalAveragePooling2D  
Modelo compacto, menos parámetros.

---

## 4. Comparación de Modelos

La comparación se basó en **val_accuracy**.

| Modelo | Mejor val_accuracy | Época |
|--------|---------------------|--------|
| **Model 3** | **0.8240** | **5** |
| Model 5 | 0.7908 | 6 |
| Baseline | 0.7887 | 7 |
| Model 2 | 0.7257 | 7 |
| Model 6 | 0.4310 | 6 |
| Model 4 | 0.1017 | 2 |

**Modelo ganador:** **Model 3**, por su mayor capacidad de aprendizaje sin caer en sobreajuste temprano.

---

# 5. Gráficas del Modelo Ganador

### Exactitud (Train vs Validation)
La exactitud de validación se estabiliza alrededor de **0.82** después de la época 3.

### Pérdida (Train vs Validation)
La pérdida de validación deja de mejorar desde la época 3–4, indicando el punto óptimo de entrenamiento.

---

# #6. Reentrenamiento con Todo el Dataset + Evaluación Final

El modelo ganador fue reconstruido y reentrenado con **100% del conjunto de entrenamiento**.

Resultados del conjunto **Test (2064 imágenes)**:

- **Accuracy global:** **0.91**  
- Mejores clases: **3, 7, 8, 9**  
- Clases con f1 más bajo pero estable: **1 y 4**  
- Buen balance entre precisión, recall y f1-score.

---

## 7. Clasificación en Tiempo Real (OpenCV)

Se desarrolló un sistema de clasificación en vivo:

- Captura mediante **webcam**.  
- Procesamiento tipo OCR:  
  - Escala de grises  
  - Blur  
  - Threshold (Otsu + invertido)  
  - Dilatación y erosión  
  - Detección de contornos  
- Cada dígito detectado se:
  - Recorta  
  - Redimensiona a 280×280  
  - Normaliza  
  - Clasifica con el modelo  
- Se muestran:
  - **Probabilidades por clase** (verde = clase ganadora).  
  - **Bounding boxes** sobre los dígitos detectados.  
  - Reconocimiento de **múltiples dígitos a la vez**.

---

## 8. OCR Multidígito 

El sistema también admite:

- Detección de varios contornos en un mismo frame.  
- Clasificación independiente de cada región.  
- Visualización simultánea de todas las predicciones.  

Esto extiende el modelo a escenarios reales como notas manuscritas o pizarrones.

---

# Archivos del Proyecto

- [Archivo ipynb del proyecto](P3_648241.ipynb)  
- [Archivo HTML del proyecto](P3_648241.html)  
- [Archivo ipynb del tiempo real](P3J_648241.ipynb)  
- [Archivo HTML del tiempo real](P3J_648241.html)   
