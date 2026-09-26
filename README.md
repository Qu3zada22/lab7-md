# Laboratorio 7 — Spark MLlib con la ENEIC

CC3066 – Data Science · Universidad del Valle de Guatemala

Integrantes: Iris Ayala, Jonathan Díaz y Anggie Quezada

Todo el trabajo está en `lab7_eneic.ipynb`. Las instrucciones del laboratorio están en `docs/`.

## Requisitos

- Python 3.11
- Java 17 (Spark 3.5 es compatible con Java 8, 11 y 17)
- PySpark 3.5.1 y las librerías de `requirements.txt`

### Instalar Java 17

macOS (Homebrew):

```
brew install openjdk@17
```

Linux (Ubuntu/Debian):

```
sudo apt install openjdk-17-jdk
```

En macOS y Linux el notebook encuentra Java 17 automáticamente; no hace falta configurar `JAVA_HOME`.

Windows: se recomienda usar WSL2 (Ubuntu) y seguir los pasos de Linux.

## Preparar el entorno

Desde la carpeta del proyecto, en macOS, Linux o WSL:

```
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt "pandas<3" "numpy<2" "pyarrow<18"
python -m ipykernel install --user --name lab7-venv --display-name "Python (.venv lab7)"
```

## Datos

Los archivos de datos no están en el repositorio. Se descargan de la página de la ENEIC del INE:
https://www.ine.gob.gt/encuesta-nacional-de-empleo-e-ingresos/

Solo se usan las bases de Personas. Deben guardarse en `data/` con estos nombres exactos:

| Trimestre | Archivo                                      |
| --------- | -------------------------------------------- |
| I 2025    | `Personas_ENEIC_T1_2025.xlsx`                |
| II 2025   | `Personas-ENEIC-T2-2025.xlsx`                |
| III 2025  | `Base-de-datos-Personas-ENEIC-III-2025.xlsx` |
| IV 2025   | `Base-de-datos-Personas-ENEIC-IV-2025.xlsx`  |
| I 2026    | `Base-de-datos-Personas-ENEIC-I-2026.xlsx`   |

El diccionario `data/Diccionario-Personas-ENEIC-I-2026.xlsx` sí está en el repositorio.

## Ejecutar

```
source .venv/bin/activate
jupyter lab
```

El notebook genera dos carpetas que no se suben al repositorio:

- `data/parquet/`: los Excel convertidos a Parquet. La primera ejecución los crea; las siguientes los reutilizan y son más rápidas.
- `modelos/`: los mejores modelos guardados (`lr_mejor` y `rf_mejor`).

## Avance

| Ejercicio                                             | Estado    |
| ----------------------------------------------------- | --------- |
| 1. Carga, armonización y calidad de datos             | Listo     |
| 2. Estadística descriptiva y preguntas de exploración | Listo     |
| 3. Relaciones entre variables numéricas               | Listo     |
| 4. Segmentación de perfiles mediante KMeans           | Listo     |
| 5. Pipeline de regresión lineal                       | Listo     |
| 6. Pipeline de Random Forest                          | Listo     |
| 7. Entrenamiento final y evaluación en 2026           | Pendiente |
| 8. Visualización y análisis de errores                | Pendiente |

### Lo que falta

Ejercicio 7:

- Volver a entrenar las dos configuraciones elegidas con todo 2025 (`train_full`), usando `pipeline_lr(**mejor_lr)` y `pipeline_rf(**mejor_rf)`.
- Predecir sobre 2026 (`test`) con los dos modelos, sobre exactamente los mismos registros.
- Comparar MAE, RMSE y R² de los dos modelos y del modelo de referencia (media del salario de `train_full`) con la función `evaluar()`.

Ejercicio 8:

- Para cada modelo, una gráfica de salario real contra predicho con la línea y = x, y una de residuos contra predicho con una línea horizontal en 0. Usar la misma muestra de hasta 5,000 registros para los dos modelos.
- Residuo = real − predicho (positivo = subestimación).
- Tablas de MAE, error medio y número de observaciones por nivel educativo y por dominio, con todos los registros de prueba.
- Análisis del error por percentiles de salario: ¿los modelos subestiman o sobreestiman los salarios altos?
- Discusión final con todos los hallazgos.

Variables disponibles de los ejercicios anteriores: `train`, `valid`, `train_full`, `test`, `evaluar()`, `NUMS`, `CATS`, `OBJETIVO`, `mejor_lr`, `mejor_rf`, `pipeline_lr()`, `pipeline_rf()` y `MODELOS_DIR`.
