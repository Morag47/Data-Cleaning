# Data-Cleaning
English

## Retail Store Sales — Data Cleaning Project

### Overview
This repository contains a full data cleaning process applied to a "dirty" retail sales dataset originally with 12,575 transactions across 8 product categories (25 items per category). The raw data included intentional inconsistencies — missing prices, incomplete items, mixed decimal formats, and incomplete transactions — designed to simulate real-world data quality problems. The final cleaned file contains 11,971 fully valid transactions.

### Repository Contents
- `retail_store_sales.csv` — original dataset (source: Kaggle)
- `CleaningV4.xlsx` — final cleaned dataset, containing two sheets:
  - **retail_store_sales**: the cleaned transactional data
  - **product_lookup**: reference table used to recover missing item names via merge

### Tools Used
- Excel 365
- Power Query (M language) for transformations and merges
- Conditional logic formulas for price correction
- Lookup/reference tables for data recovery via merge

### Step-by-Step Cleaning Process

**Step 1 — Diagnosing the raw data**
Explored the dataset structure and identified four major issues: inconsistent price formats (values like `185` instead of `18.5`, or `6,5` instead of `6.5`), missing `Item` values in ~4.8% of rows, incomplete transactions missing both `Quantity` and `Total Spent`, and missing `Discount Applied` values in ~33% of rows.

**Step 2 — Correcting price inconsistencies**
Confirmed the real price range for all products: minimum `5.0`, maximum `41.0`. Built a conditional rule in Power Query: values already in decimal format were kept as-is; integer values above the maximum threshold were divided by 10 to bring them back to the correct scale. Result: column `Final_Price_Per_Unit`, 100% populated and consistent with the known price range.

**Step 3 — Deducing missing items**
Built a reference lookup table (**product_lookup** sheet) mapping every combination of `Category` + `Final_Price_Per_Unit` to its corresponding item code, covering all 8 categories × 25 items = 200 unique products. Merged this reference table against the main dataset using `Category` and `Final_Price_Per_Unit` as keys. Validated the merge at a 100% match rate, without generating duplicate rows.

**Step 4 — Handling incomplete transactions**
Identified 604 rows (≈4.8% of the original dataset) missing both `Quantity` and `Total Spent` simultaneously. Verified there was no mathematical way to reconstruct these values, since `Total Spent = Quantity × Price Per Unit` requires at least one of the two values to be present to solve for the other. Removed these rows rather than fabricating values, leaving the final dataset with 11,971 rows.

**Step 5 — The Discount Applied decision (intentional exception)**
Found `Discount Applied` missing in roughly a third of transactions. Analyzed the distribution of known values: close to a 50/50 split between discount applied and not applied. Concluded that imputing a fixed value would introduce artificial bias, since no other column can reliably predict this flag. Decision: left these values as missing intentionally, choosing data honesty over artificial completeness.

**Step 6 — Final column cleanup**
Removed intermediate/helper columns once their purpose was fulfilled: the original uncorrected price column, an intermediate deduced-price column, and an intermediate item column — all superseded by their final, validated versions.

### Final Dataset Structure

| Column | Description |
|---|---|
| Transaction ID | Unique transaction identifier |
| Customer ID | Unique customer identifier (25 customers) |
| Category | Product category (8 total) |
| Item_Final_Completed | Deduced/completed item code |
| Final_Price_Per_Unit | Corrected unit price (range: 5.0–41.0) |
| Quantity | Units purchased |
| Total Spent | Quantity × Final_Price_Per_Unit |
| Payment Method | Cash, Credit Card, Digital Wallet |
| Location | In-store or Online |
| Transaction Date | Date of transaction |
| Discount Applied | True/False, blank = Unknown (intentionally left unresolved) |

### Key Takeaways
Cleaning data isn't just filling gaps — it's knowing which gaps can be filled responsibly and which should be left transparent. Business logic (a known price range) can correct data even without an explicit formula, as long as the reasoning is validated against the full dataset. A near-50/50 distribution in missing categorical data is a strong signal that imputation would bias the dataset — sometimes the most professional decision is to leave a value unknown.

### License
Original dataset released under CC BY-SA 4.0 (Kaggle).

***

# Español

## Proyecto de Limpieza de Datos — Ventas de Tienda Retail

### Resumen
Este repositorio contiene un proceso completo de limpieza de datos aplicado a un dataset de ventas retail "sucio", originalmente con 12,575 transacciones distribuidas en 8 categorías de productos (25 ítems por categoría). Los datos originales incluían inconsistencias intencionales: precios faltantes, ítems incompletos, formatos decimales mezclados y transacciones incompletas, diseñadas para simular problemas reales de calidad de datos. El archivo final limpio contiene 11,971 transacciones completamente válidas.

### Contenido del Repositorio
- `retail_store_sales.csv` — dataset original (fuente: Kaggle)
- `CleaningV4.xlsx` — dataset final limpio, con dos hojas:
  - **retail_store_sales**: los datos transaccionales limpios
  - **product_lookup**: tabla de referencia usada para recuperar nombres de ítems faltantes mediante merge

### Herramientas Utilizadas
- Excel 365
- Power Query (lenguaje M) para transformaciones y combinaciones (merge)
- Fórmulas de lógica condicional para corrección de precios
- Tablas de referencia (lookup) para recuperación de datos mediante merge

### Proceso de Limpieza Paso a Paso

**Paso 1 — Diagnóstico de los datos crudos**
Se exploró la estructura del dataset y se identificaron cuatro problemas principales: formatos de precio inconsistentes (valores como `185` en lugar de `18.5`, o `6,5` en lugar de `6.5`), valores de `Item` faltantes en ~4.8% de las filas, transacciones incompletas sin `Quantity` ni `Total Spent`, y valores de `Discount Applied` faltantes en ~33% de las filas.

**Paso 2 — Corrección de inconsistencias de precio**
Se confirmó el rango real de precios de todos los productos: mínimo `5.0`, máximo `41.0`. Se construyó una regla condicional en Power Query: los valores que ya venían en formato decimal se dejaron igual; los valores enteros por encima del umbral máximo se dividieron entre 10 para llevarlos a la escala correcta. Resultado: columna `Final_Price_Per_Unit`, completa al 100% y consistente con el rango de precios conocido.

**Paso 3 — Deducción de ítems faltantes**
Se construyó una tabla de referencia (hoja **product_lookup**) que mapea cada combinación de `Category` + `Final_Price_Per_Unit` con su código de ítem correspondiente, cubriendo las 8 categorías × 25 ítems = 200 productos únicos. Se hizo el merge de esta tabla de referencia contra el dataset principal usando `Category` y `Final_Price_Per_Unit` como claves. Se validó el merge con una tasa de coincidencia del 100%, sin generar filas duplicadas.

**Paso 4 — Manejo de transacciones incompletas**
Se identificaron 604 filas (≈4.8% del dataset original) sin `Quantity` ni `Total Spent` al mismo tiempo. Se verificó que no había forma matemática de reconstruir estos valores, ya que `Total Spent = Quantity × Price Per Unit` requiere que al menos uno de los dos valores esté presente para poder despejar el otro. Se eliminaron estas filas en lugar de inventar valores, dejando el dataset final con 11,971 filas.

**Paso 5 — La decisión sobre Discount Applied (excepción intencional)**
Se encontró que `Discount Applied` estaba vacío en aproximadamente un tercio de las transacciones. Se analizó la distribución de los valores conocidos: una división cercana a 50/50 entre descuento aplicado y no aplicado. Se concluyó que imputar un valor fijo introduciría un sesgo artificial, ya que ninguna otra columna puede predecir esta bandera de forma confiable. Decisión: se dejaron estos valores como faltantes intencionalmente, priorizando la honestidad de los datos sobre una completitud artificial.

**Paso 6 — Limpieza final de columnas**
Se eliminaron las columnas intermedias/auxiliares una vez cumplido su propósito: la columna de precio original sin corregir, una columna intermedia de precio deducido, y una columna intermedia de ítem — todas reemplazadas por sus versiones finales ya validadas.

### Estructura Final del Dataset

| Columna | Descripción |
|---|---|
| Transaction ID | Identificador único de transacción |
| Customer ID | Identificador único de cliente (25 clientes) |
| Category | Categoría del producto (8 en total) |
| Item_Final_Completed | Código de ítem deducido/completado |
| Final_Price_Per_Unit | Precio unitario corregido (rango: 5.0–41.0) |
| Quantity | Unidades compradas |
| Total Spent | Quantity × Final_Price_Per_Unit |
| Payment Method | Efectivo, Tarjeta de Crédito, Billetera Digital |
| Location | En tienda o En línea |
| Transaction Date | Fecha de la transacción |
| Discount Applied | Verdadero/Falso, vacío = Desconocido (dejado sin resolver intencionalmente) |

### Aprendizajes Clave
Limpiar datos no es solo llenar huecos: es saber cuáles huecos se pueden llenar de forma responsable y cuáles deben quedar transparentes. La lógica de negocio (un rango de precios conocido) puede corregir datos incluso sin una fórmula explícita, siempre que el razonamiento esté validado contra todo el dataset. Una distribución cercana a 50/50 en datos categóricos faltantes es una señal fuerte de que la imputación sesgaría el dataset — a veces la decisión más profesional es dejar un valor como desconocido.

### Licencia
Dataset original publicado bajo licencia CC BY-SA 4.0 (Kaggle).
