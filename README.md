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

---

## 📡 Detalle del Módulo de Adquisición (01_adquisicion_datos)
Este directorio contiene los scripts Python encargados del Pipeline ETL para construir el dataset crudo a partir de la red de sensores LISN:

*  descargaDATOSANUAL.py (Extracción):

Implementa el cliente jrodb para conectar con el servidor CKAN del Radio Observatorio de Jicamarca.

Automatiza la descarga masiva de logs anuales y mensuales de estaciones GNSS específicas (Jicamarca, Piura, Huancayo, etc.).

* descomprimirDATOS.py (Transformación Física):

Script de automatización que recorre recursivamente los directorios descargados.

Ejecuta la descompresión por lotes de archivos binarios/logs (.gz, .Z) preparándolos para el procesamiento.

* generarDATASET.py (Parsing y Estructuración):

Parser Septentrio: Lee e interpreta la estructura de los logs de receptores GNSS.

Conversión Temporal: Transforma formatos de tiempo nativos a datetime estándar UTC.

Extracción de Features: Filtra y extrae las variables críticas (S4, Azimuth, Elevacion, ID_Satelite) y consolida millones de registros en un único archivo CSV ("Data Cruda") listo para la fase de Ciencia de Datos.

## 🛠️ Instalación y Requisitos
Para ejecutar los cuadernos de este repositorio, se requiere un entorno de Python 3.12+ con las siguientes librerías principales:

Bash

pip install tensorflow numpy pandas matplotlib scikit-learn seaborn scipy

## ✒️ Autor

Alexander Olmedo Valdez Portocarrero

Maestría en Ciencias de la Computación

Universidad Nacional de Ingeniería (UNI) - Lima, Perú.