# Análisis Topológico del Comportamiento de Crédito

## Descripción del Proyecto

Este proyecto realiza un análisis de datos utilizando **Topological Data Analysis (TDA)** sobre un dataset de transacciones con tarjeta de crédito.

El objetivo principal es identificar:

- Patrones no lineales en el comportamiento de compra.
- Posibles estructuras topológicas complejas.
- Segmentaciones naturales de clientes según:
  - Ciudad
  - Tipo de tarjeta
  - Tipo de gasto
  - Género
  - Monto de transacción (`Amount`)

Se combinan técnicas tradicionales de preprocesamiento con herramientas avanzadas como:

- Persistent Homology (Ripser / Gudhi)
- Mapper (KeplerMapper)
- PCA
- Clustering

---

## Preguntas de Investigación

1. ¿Existen patrones no lineales o estructuras topológicas complejas en el comportamiento de compra?
2. ¿Se pueden identificar segmentos claros de clientes en función de sus características demográficas y monto de gasto?

---

## Dataset

El análisis utiliza el archivo:

```
credit_card_data.csv
```

Columnas relevantes:

- `City`
- `Card Type`
- `Exp Type`
- `Gender`
- `Amount`
- `Date`

---

## Pipeline del Análisis

### 1. Carga y Limpieza de Datos

- Eliminación de la columna `index`
- Análisis exploratorio de variables categóricas
- Reducción de ciudades a las 4 más frecuentes para reducir dimensionalidad y carga computacional

---

### 2. Análisis Exploratorio

- Histogramas de la variable `Amount`
- Observación de valores atípicos (hasta ~1,000,000)
- Identificación de distribución sesgada

---

### 3. Encoding

Se utilizó:

**Target Encoding** para:
- `City`
- `Card Type`
- `Exp Type`

Este método se eligió porque preserva mejor la estructura topológica respecto al target (`Amount`).

**Label Encoding** para:
- `Gender`

---

### 4. Scaling

Se aplicó `StandardScaler` a:

- `Amount`
- Variables codificadas

Esto es crucial porque:

- PCA es sensible a la escala.
- TDA depende de distancias métricas.
- Mapper utiliza particiones basadas en proximidad.

---

## Topological Data Analysis (TDA)

### Persistent Homology

Se aplicó:

- Reducción dimensional con PCA (3 componentes)
- Cálculo de:
  - H₀ (componentes conexos)
  - H₁ (loops)
- Visualización con:
  - Persistence Barcode
  - Persistence Diagram

**Resultados clave:**

- Muy pocos componentes con persistencia significativa.
- No se detectaron estructuras topológicas complejas fuertes.
- H₁ muestra ciclos de muy corta duración.

Conclusión parcial: la estructura del dataset es relativamente simple.

---

### Mapper (KeplerMapper)

Se construyó un grafo topológico utilizando:

- Variables codificadas
- Mayor número de cubos
- Menor overlap

Objetivo: detectar si los valores altos de `Amount` forman clusters independientes.

**Resultados:**

- Los valores altos no forman clusters separados.
- Se integran en clusters centrales bien conectados.
- No se identifican regiones topológicamente aisladas.

---

## Resultados

- No se encontraron patrones topológicos complejos.
- Existen ligeras diferencias en hábitos de consumo.
- Métodos tradicionales como KMeans serían suficientes para segmentación.
- TDA confirma que la estructura del espacio es relativamente simple.

---

## Librerías Utilizadas

```python
pandas
numpy
matplotlib
scikit-learn
ripser
persim
gudhi
kmapper
```

---

## Cómo Ejecutar

1. Instalar dependencias:

```bash
pip install pandas numpy matplotlib scikit-learn ripser persim gudhi kmapper
```

2. Colocar el archivo `credit_card_data.csv` en el mismo directorio.

3. Ejecutar el notebook:

```bash
jupyter notebook Analisis_topologico_credito.ipynb
```

---

## Conclusión General

El comportamiento de compra:

- Presenta variabilidad.
- No muestra estructuras topológicas profundas.
- Puede modelarse adecuadamente con técnicas clásicas de clustering.

El uso de TDA permitió validar que no existen configuraciones geométricas ocultas significativas en los datos.
