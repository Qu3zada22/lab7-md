# Laboratorio 7 — Spark MLlib con la ENEIC

CC3066 – Data Science · Universidad del Valle de Guatemala

Integrantes: Iris Ayala, Jonathan Díaz y Anggie Quezada

**Repositorio Git:** <https://github.com/Qu3zada22/lab7-md>

Todo el trabajo está en `lab7_eneic.ipynb`. Las instrucciones del laboratorio están en `docs/`.

## Requisitos

- Python 3.11
- Java 17 (Spark 3.5 es compatible con Java 8, 11 y 17)
- [uv](https://docs.astral.sh/uv/)
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
uv python install 3.11
uv venv --python 3.11
uv run --python 3.11 --with-requirements requirements.txt python -c "import pyspark,pandas,numpy,pyarrow,openpyxl,matplotlib,seaborn; print(pyspark.__version__)"
```

No es necesario activar el entorno: `uv run` usa `.venv` y resuelve las dependencias declaradas en `requirements.txt`.

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

## Ejecutar de forma interactiva

```
uv run --python 3.11 --with-requirements requirements.txt jupyter lab
```

## Ejecutar y verificar el notebook completo

Cuando estén disponibles las cinco bases, ejecutar:

```
uv run --python 3.11 --with-requirements requirements.txt jupyter nbconvert --to notebook --execute lab7_eneic.ipynb --output lab7_eneic.executed.ipynb --output-dir /tmp --ExecutePreprocessor.timeout=-1
```

El archivo ejecutado se escribe en `/tmp` para no generar una salida temporal versionable dentro del repositorio. Las cinco bases deben estar disponibles en `data/` antes de ejecutar el comando.

El notebook genera dos carpetas que no se suben al repositorio:

- `data/parquet/`: los Excel convertidos a Parquet. La primera ejecución los crea; las siguientes los reutilizan y son más rápidas.
- `modelos/`: los modelos de selección (`lr_mejor` y `rf_mejor`) y los modelos finales (`lr_final_2025` y `rf_final_2025`).

## Avance

| Ejercicio                                             | Estado    |
| ----------------------------------------------------- | --------- |
| 1. Carga, armonización y calidad de datos             | Listo     |
| 2. Estadística descriptiva y preguntas de exploración | Listo     |
| 3. Relaciones entre variables numéricas               | Listo     |
| 4. Segmentación de perfiles mediante KMeans           | Listo     |
| 5. Pipeline de regresión lineal                       | Listo     |
| 6. Pipeline de Random Forest                          | Listo     |
| 7. Entrenamiento final y evaluación en 2026           | Listo     |
| 8. Visualización y análisis de errores                | Listo     |

### Resultados ejecutados

El notebook conserva los resultados de una ejecución integral con Python 3.11. Los modelos finales se entrenaron con los cuatro trimestres de 2025 y se evaluaron sobre los mismos 13,258 registros elegibles del primer trimestre de 2026.

- La actividad 7 compara la referencia, la regresión lineal y Random Forest con MAE, RMSE y R².
- La actividad 8 incluye cuatro gráficos sobre una muestra común y reproducible de 5,000 registros.
- Las tablas por nivel educativo, dominio y percentiles salariales usan todo el conjunto de prueba; la discusión final documenta los patrones de error y las limitaciones observadas.
