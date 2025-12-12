# Tesis de Maestría: Sistema de Pronóstico del Centelleo Ionosférico (S4) con Deep Learning

![Python](https://img.shields.io/badge/Python-3.12%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.19-orange)
![Status](https://img.shields.io/badge/Status-Tesis_Desarrollo-green)
![Institution](https://img.shields.io/badge/Universidad-UNI-red)

> **Título:** DESARROLLO DE UN SISTEMA DE PRONÓSTICO DEL CENTELLEO IONOSFÉRICO SOBRE EL PERÚ UTILIZANDO MACHINE LEARNING PARA LA MITIGACIÓN DE PERTURBACIONES EN SEÑALES SATÉLITES.

## 📄 Descripción del Proyecto

Este repositorio contiene el código fuente, los cuadernos de experimentación y la documentación técnica desarrollada para la obtención del grado de **Maestro en Ciencias de la Computación** en la **Universidad Nacional de Ingeniería (UNI)**.

El proyecto aborda la problemática del **centelleo ionosférico** en la región ecuatorial, un fenómeno que degrada la precisión de los sistemas GNSS (GPS, Galileo, etc.). Se propone una solución basada en **Deep Learning** para predecir el índice $S_4$ con un horizonte de alerta temprana.

### 🚀 Innovación Técnica
El sistema implementa una arquitectura **Sequence-to-Sequence (Seq2Seq)** avanzada con las siguientes características:
* **Enfoque Multi-Step:** Predicción de vectores de secuencias futuras (20 minutos) en lugar de puntos aislados.
* **Arquitectura Bi-LSTM:** Redes Bidireccionales Encoder-Decoder para capturar el contexto temporal completo.
* **Estrategia "Worst-Case":** Preprocesamiento inteligente que agrega la señal del satélite más crítico visible en el cielo.
* **Weighted Focal Loss:** Una función de costo personalizada diseñada para penalizar la subestimación de eventos extremos en datasets desbalanceados.

---

## 📂 Estructura del Repositorio

El proyecto está organizado en tres módulos principales para facilitar la reproducibilidad científica:

```text
├── 📁 documentos/
│   └── Tesis_Maestria_Borrador.docx  # Documento formal de la tesis (Versión Word)
│
├── 📁 01_adquisicion_datos/
│   │   # Módulo ETL para la base de datos LISN/IGP
│   ├── descargaDATOSANUAL.py
│   ├── descomprimirDATOS.py
│   ├── generarDATASET.py
│   │
│   └── notebooks_exploratorios/
│       └── descargar_data_estaciones.ipynb
│
├── 📁 02_metodologia_ds/
│   ├── 📁 experimentos_preliminares/
│   │   └── S2_MAESTRIA_S4_LSTM_13072025.ipynb
│   ├── N1_S4_MAESTRIA_PROCESAMIENTO.ipynb      # Benchmark de Arquitecturas (Single-Step)
│   ├── 02_entrenamiento_modelos.ipynb
│   └── 03_evaluacion_pruebas.ipynb
│   # Pipeline completo de Ciencia de Datos:
│   # - Limpieza: Filtros de elevación (>30°) y outliers.
│   # - Preprocesamiento: Agregación "Worst-Case" y generación de tensores.
│   # - Modelado: Implementación de Bi-LSTM Multi-Step y Loss Asimétrica.
│   # - Pruebas: Validación cruzada y métricas de eventos (Event-RMSE).
│
└── README.md
```
---

## 📡 Detalle del Módulo de Adquisición (01_adquisicion_datos)
Este directorio contiene los scripts Python encargados del Pipeline ETL para construir el dataset crudo a partir de la red de sensores LISN:

*  **descargaDATOSANUAL.py (Extracción):**

Implementa el cliente jrodb para conectar con el servidor CKAN del Radio Observatorio de Jicamarca.

Automatiza la descarga masiva de logs anuales y mensuales de estaciones GNSS específicas (Jicamarca, Piura, Huancayo, etc.).

* **descomprimirDATOS.py (Transformación Física):**

Script de automatización que recorre recursivamente los directorios descargados.

Ejecuta la descompresión por lotes de archivos binarios/logs (.gz, .Z) preparándolos para el procesamiento.

* **generarDATASET.py (Parsing y Estructuración):**

Parser Septentrio: Lee e interpreta la estructura de los logs de receptores GNSS.

Conversión Temporal: Transforma formatos de tiempo nativos a datetime estándar UTC.

Extracción de Features: Filtra y extrae las variables críticas (S4, Azimuth, Elevacion, ID_Satelite) y consolida millones de registros en un único archivo CSV ("Data Cruda") listo para la fase de Ciencia de Datos.

## 📓 Cuadernos de Experimentación y Prototipado

* **`S2_MAESTRIA_S4_LSTM_13072025.ipynb` (Prototipo LSTM Base):**
    Este cuaderno documenta la **fase exploratoria inicial** del modelado predictivo. Su objetivo fue establecer la línea base de rendimiento utilizando una arquitectura LSTM estándar ("Vanilla") antes de evolucionar hacia modelos híbridos o multi-step.
    
    **Puntos clave abordados:**
    * **Sintonización Temporal:** Evaluación de diferentes configuraciones de ventana deslizante (*Window Size*) de 2 a 3 horas frente a horizontes de predicción de 20, 30 y 40 minutos.
    * **Detección de Fenómenos de Lag:** Identificación del problema de retraso de fase cuando se utilizan ventanas históricas excesivamente largas (>3 horas) sin mecanismos de atención.
    * **Análisis de Sensibilidad:** Pruebas preliminares sobre el impacto de la normalización y la estructura de las secuencias en la convergencia de la red.
    * *Nota:* Este archivo sirvió como base empírica para refinar la estrategia de "Gap Control" y definir el horizonte óptimo de 20 minutos utilizado en la versión final del sistema.

* **`N1_S4_MAESTRIA_PROCESAMIENTO.ipynb` (Benchmark de Arquitecturas - Single Step):**
    Este cuaderno implementa la fase de **validación arquitectónica** utilizando el dataset completo. Se centra en una estrategia de predicción de **un solo paso (Single-Step Forecasting)** para aislar y comparar el rendimiento puro de las diferentes topologías neuronales.
    
    **Aportes principales:**
    * **Benchmark Comparativo:** Evaluación rigurosa entre tres modelos:
        1.  **Baseline:** LSTM Simple (Vanilla).
        2.  **Robust:** Weighted LSTM (con función de costo personalizada).
        3.  **Hybrid:** Morph-LSTM-ELM (Propuesta inicial con regresión analítica).
    * **Validación de la Función de Costo:** Demuestra empíricamente cómo la *Loss Asimétrica* reduce el error en los picos ($S_4 > 0.6$) comparado con el MSE estándar.
    * **Resultados:** Genera las métricas comparativas (RMSE Global vs. Event-RMSE) que justifican la elección de la arquitectura híbrida para la fase final.

* **`N2_S4_MAESTRIA_PROCESAMIENTO.ipynb` (Pipeline Final - Pronóstico Multi-Step):**
    Este cuaderno contiene la **implementación definitiva de la propuesta de tesis**. A diferencia de los enfoques anteriores, aquí se despliega una arquitectura **Sequence-to-Sequence (Seq2Seq)** capaz de predecir una trayectoria futura completa (vector de 20 minutos) en lugar de un solo punto.
    
    **Características Avanzadas Implementadas:**
    * **Generación de Tensores Multi-Step:** Algoritmo de ventaneo inteligente con validación de huecos (*Gap Control*), asegurando continuidad temporal estricta en las secuencias de entrada ($X$) y salida ($Y$).
    * **Arquitectura Encoder-Decoder:** Implementación de una red **Bi-LSTM Profunda** (Bidireccional) con capas de *BatchNormalization* para estabilizar el aprendizaje de secuencias largas.
    * **Weighted Focal Loss (Híbrida):** Integración matemática de la función de costo asimétrica que penaliza la subestimación y focaliza el gradiente en eventos difíciles ($S_4 > 0.6$).
    * **Evaluación de Trayectorias:** Visualización y métricas de rendimiento sobre vectores completos, permitiendo analizar la coherencia de fase y la capacidad del modelo para anticipar la morfología de la tormenta.

## 🛠️ Instalación y Requisitos
Para ejecutar los cuadernos de este repositorio, se requiere un entorno de Python 3.12+ con las siguientes librerías principales:

Bash

pip install tensorflow numpy pandas matplotlib scikit-learn seaborn scipy

## ✒️ Autor

Alexander Olmedo Valdez Portocarrero

Maestría en Ciencias de la Computación

Universidad Nacional de Ingeniería (UNI) - Lima, Perú.