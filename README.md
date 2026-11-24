# Exploración y Clasificación Acústica de Géneros Musicales

## Integrante
- Benjamín Neira Provoste

## Asignatura
- **ACUS220 — Acústica Computacional con Python** (Universidad Austral de Chile)

## Idea del proyecto y pregunta principal
Construir un flujo de análisis y clasificación computacional de canciones por género musical mediante representaciones tiempo–frecuencia y extracción de características acústicas.

**Pregunta principal:** ¿Qué características acústicas y representaciones (espectrogramas log-Mel, descriptores espectrales y MFCC) permiten describir y diferenciar mejor los distintos géneros musicales?

## Datos (Dataset GTZAN)
- **Fuente:** Dataset público [GTZAN (Kaggle)](https://www.kaggle.com/datasets/andradaolteanu/gtzan-dataset-music-genre-classification).
- **Estructura auditada:** 1.000 pistas `.wav` (~30 s cada una, frecuencia de muestreo original de 22.050 Hz), perfectamente balanceadas en **10 géneros** (100 archivos por clase: *blues, classical, country, disco, hiphop, jazz, metal, pop, reggae, rock*).
- **Gestión en repositorio:** Los audios originales se mantienen localmente en `data/gtzan/` y están excluidos del control de versiones mediante `.gitignore` por restricciones de tamaño.

## Evolución y notebooks desarrollados
1. `notebooks/01_espectrograma.ipynb`: Prueba inicial de entorno (`acus220_2026`) y primer espectrograma (Hito 1).
2. `notebooks/02_auditoria_datos.ipynb`: Auditoría automatizada del dataset GTZAN con `pathlib` y `pandas`, confirmando balance de clases.
3. `notebooks/03_exploracion_visual.ipynb`: Pipeline modular de inspección visual comparando forma de onda y espectrogramas log-Mel (`n_fft=2048`, `hop_length=512`, `n_mels=128`) entre géneros contrastantes (*Classical* vs. *Metal*).
4. `notebooks/04_extraccion_caracteristicas.ipynb`: Extracción de características escalares y espectrales (RMS, ZCR, centroide espectral, ancho de banda y MFCC) en 4 géneros contrastantes, más evaluación crítica de `librosa.pyin` ($F_0$) en música polifónica.

## Decisiones metodológicas clave (Hito 2)
- **Parámetros STFT/Mel adaptados a música:** Se aumentó la resolución respecto a señales de habla (`n_fft=2048`, `hop_length=512`, `n_mels=128`) para adecuarse a $f_s = 22.050\text{ Hz}$ y capturar mejor la riqueza armónica.
- **Descarte de $F_0$ (`librosa.pyin`) como descriptor principal:** Las pruebas demostraron que, al tratarse de audio polifónico (múltiples instrumentos simultáneos y distorsión), la estimación de frecuencia fundamental se fragmenta o salta entre fuentes sonoras, por lo que no resulta confiable para clasificar géneros.
- **Priorización de MFCC y descriptores espectrales:** Las métricas de energía (RMS), brillo (centroide/ZCR) y timbre (MFCC) muestran separación clara entre géneros acústicos y densos, aunque géneros cercanos (*Classical* vs. *Jazz*) requieren combinar múltiples características.

## Pipeline actualizado
```text
[Completado] Adquisición y aislamiento del dataset GTZAN (.gitignore)
      ↓
[Completado] Auditoría de clases y metadatos (pandas / pathlib)
      ↓
[Completado] Exploración visual comparativa (forma de onda y log-Mel)
      ↓
[Completado] Extracción de características (RMS, ZCR, espectrales, MFCC) y descarte justificado de F0
      ↓
[Hito 3]     Extracción masiva de características sobre las 1.000 pistas (CSV derivado)
      ↓
[Hito 3]     Entrenamiento y evaluación de clasificadores base (ej. Random Forest / SVM) con matriz de confusión