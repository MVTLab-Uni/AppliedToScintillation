# Tesis de Maestría: Sistema de Pronóstico del Centelleo Ionosférico (S4) con Deep Learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![Status](https://img.shields.io/badge/Status-Tesis_Finalizada-green)
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
│   ├── descargar_data_estaciones.ipynb
│   ├── parser_omniweb.py
│   └── generador_dataset_crudo.py
│   # Scripts para:
│   # - Conexión con LISN, OMNIWeb y ROJ.
│   # - Descarga de logs de receptores GNSS.
│   # - Consolidación del Dataset de "Data Cruda" multisatelital.
│
├── 📁 02_metodologia_ds/
│   ├── 01_limpieza_preprocesamiento.ipynb
│   ├── 02_entrenamiento_modelos.ipynb
│   └── 03_evaluacion_pruebas.ipynb
│   # Pipeline completo de Ciencia de Datos:
│   # - Limpieza: Filtros de elevación (>30°) y outliers.
│   # - Preprocesamiento: Agregación "Worst-Case" y generación de tensores.
│   # - Modelado: Implementación de Bi-LSTM Multi-Step y Loss Asimétrica.
│   # - Pruebas: Validación cruzada y métricas de eventos (Event-RMSE).
│
└── README.md