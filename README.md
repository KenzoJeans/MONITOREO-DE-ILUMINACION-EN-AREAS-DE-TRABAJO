# Monitoreo de Iluminación en Áreas de Trabajo – Kenzo Jeans

Dashboard para el seguimiento de las mediciones de iluminancia (lux) en las áreas de trabajo de Kenzo Jeans, comparadas con los niveles mínimos del **RETILAP** (Reglamento Técnico de Iluminación y Alumbrado Público, Colombia).

## Propósito

- Registrar las mediciones de iluminación de cada área sin papel.
- Verificar automáticamente si cada medición **cumple** con el nivel mínimo y la uniformidad esperados.
- Identificar las áreas con deficiencia de iluminación y ver su evolución en el tiempo.
- Exportar los resultados a Excel para informes y auditorías.

## Cómo fluye la información

```
Formulario de Google  →  Hoja de cálculo (Google Sheets)  →  Dashboard (Streamlit)
```

1. Quien mide diligencia el formulario con el área, el clima y las lecturas de los puntos de medición.
2. Las respuestas llegan a la hoja de cálculo.
3. El dashboard descarga la hoja como CSV, calcula los indicadores y muestra los resultados.

El ID de la hoja de cálculo viene precargado en la barra lateral (campo "ID o URL de Google Sheets"). La hoja debe estar compartida como **"Cualquier persona con el enlace → Lector"**.

Si la descarga falla, el dashboard permite **subir un CSV** o **pegar su contenido** manualmente.

## Columnas que espera la hoja

Las columnas se reconocen por palabras clave en el encabezado (con tolerancia a pequeñas diferencias de escritura):

| Dato | Palabras clave en el encabezado |
|---|---|
| Marca temporal de Google Forms | `marca` + `temporal`, `timestamp` |
| Fecha de la medición | `fecha`, `date` |
| Área | `area`, `área`, `ubicación`, `zona`, `departamento`, `sede` |
| Condición climática | `clima`, `condición`, `weather` |
| Puntos de medición (hasta 8) | `(P1)` … `(P8)`, o encabezados `P1` … `P8` |

Los puntos de medición se leen como números (se acepta coma decimal). Un punto vacío o `N/A` simplemente no se cuenta.

## Cómo se calcula el cumplimiento

Para cada medición (cada fila):

| Indicador | Cálculo |
|---|---|
| Lux promedio | Promedio de P1 a P8 |
| Lux mínimo y máximo | Menor y mayor lectura entre los puntos |
| Uniformidad (U₀) | Lux mínimo ÷ lux promedio |
| **Cumple** | Lux promedio ≥ mínimo normativo del área **y** U₀ ≥ 0,6 |

Si una fila no tiene U₀ calculable, se evalúa solo por el nivel de lux. En el dashboard, el color del indicador de cumplimiento es verde con 80 % o más, amarillo entre 60 % y 80 %, y rojo por debajo de 60 %. La meta de 80 % también aparece como línea en la gráfica de cumplimiento por área.

## Niveles normativos por área

Están definidos en el diccionario `NORMA_LUX` del código (mínimo y recomendado, en lux):

| Área | Mínimo | Recomendado |
|---|---|---|
| Sistemas, Financiero, Comercial, RRHH, Tesoreria, Mercadeo, Ingenieria | 300 | 500 |
| Inventarios | 300 | 500 |
| Diseno | 500 | 750 |
| Corte, Bordado | 500 | 750 |
| Tintoreria | 200 | 500 |
| PTAR | 200 | 300 |
| Importados, Insumos | 100 | 300 |
| Cualquier otra área (valor por defecto) | 300 | 500 |

> **Importante:** el nombre del área debe coincidir **exactamente** con estas claves (sin tildes y respetando mayúsculas: `Diseno`, `Tintoreria`, `Tesoreria`, `Ingenieria`). Si el formulario escribe `Diseño` o `Tintorería`, el área no se reconoce y se evalúa con el valor por defecto (300 lux), sin ningún aviso.

> **Validación técnica:** estos valores son los que están escritos en el código. Conviene que el área responsable (SST / gestión ambiental) los revise contra la tabla vigente del RETILAP y los actualice en `NORMA_LUX` si cambian.

## Qué ofrece el dashboard

- **Filtros:** por área, condición climática y rango de fechas.
- **Indicadores:** mediciones totales, áreas evaluadas, lux promedio global, % de cumplimiento normativo y uniformidad media.
- **Gráficas:** iluminancia promedio por área, cumplimiento por área, distribución de lux por área (variabilidad entre puntos), relación entre clima e iluminancia, evolución temporal y distribución de condiciones climáticas.
- **Tablas:** vista previa de los datos filtrados (primeras 200 filas) y resumen por área.
- **Exportación a Excel:** un archivo con las hojas `Datos`, `Resumen por area` y `Meta` (fecha de generación, registros y áreas incluidas).

## Tecnologías y dependencias

Python con Streamlit, Pandas, NumPy, Plotly, Requests y openpyxl.

```
pip install streamlit pandas numpy plotly requests openpyxl
```

> Completar: confirmar que `requirements.txt` incluya estas librerías.

## Ejecución y despliegue

> Completar:

- **Archivo principal:** `app_iluminacion.py`
- **Ejecutar en local:** `streamlit run app_iluminacion.py`
- **Despliegue:** Streamlit Cloud, cuenta `[completar]`, URL `[completar]`
- **Formulario de medición (enlace de respuesta):** `[completar]`
- **Equipo de medición (luxómetro) y fecha de calibración:** `[completar]`

## Procedimiento de medición

> Completar según el procedimiento de la empresa:

- **Quién mide:** `[área / cargo]`
- **Frecuencia:** `[periodicidad]`
- **Cuántos puntos se miden por área y cómo se distribuyen:** `[completar]`

## Responsables

- **Dueño del proceso (iluminación / SST):** `[área / cargo]`
- **Mantenimiento técnico del dashboard:** `[nombre / cargo]`
- **Cuentas (GitHub, Streamlit, Google):** `[a nombre de quién]`
