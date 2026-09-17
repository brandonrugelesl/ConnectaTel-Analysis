# Análisis de Comportamiento de Clientes – ConnectaTel

Proyecto de análisis de datos para **ConnectaTel**, una empresa de telecomunicaciones con operaciones en Latinoamérica (México y Colombia), enfocado en entender el comportamiento real de uso de servicios móviles (llamadas y mensajes) de sus clientes.

## 🎯 Objetivo del proyecto

Evaluar el comportamiento de los clientes de ConnectaTel a partir de datos registrados hasta 2024, con el fin de:

- Identificar patrones de uso de llamadas y mensajes.
- Detectar comportamientos atípicos (outliers) que puedan indicar fraude, errores de registro o clientes de uso intensivo legítimo.
- Construir segmentos de clientes según **edad** y **nivel de uso** del plan.
- Traducir esos hallazgos en recomendaciones comerciales para ConnectaTel (retención, diseño de planes, campañas dirigidas).

## 📂 Datasets utilizados

| Archivo | Contenido |
|---|---|
| `plans.csv` | Catálogo de planes: nombre del plan, mensajes y minutos incluidos, GB por mes, costo mensual y costos por unidad extra (mensaje, minuto, GB). |
| `users_latam.csv` | Datos de clientes: `user_id`, nombre, apellido, edad, ciudad, fecha de registro, plan contratado y fecha de churn. |
| `usage.csv` | Detalle de uso real por evento: tipo (`call`/`text`), fecha, duración de la llamada y longitud del mensaje, asociado a cada `user_id`. |

> Los tres archivos se relacionan mediante llaves: `user_id` conecta `users_latam.csv` con `usage.csv`, y `plan` (en `users_latam.csv`) con `plan_name` (en `plans.csv`).

## 🧩 Etapas del análisis

1. **Carga y exploración** — Lectura de los 3 datasets con `pandas`, revisión de estructura, tipos de datos y vista previa (`.head()`, `.info()`).
2. **Identificación de problemas de calidad de datos** — Revisión de valores nulos, detección de *sentinels* (valores inválidos como `"?"` en `city` o `-999` en `age`), y estandarización de fechas de registro.
3. **Limpieza de datos** — Corrección de sentinels y fechas imposibles:
   - `age`: los valores `-999` se reemplazaron por la mediana.
   - `city`: los valores `"?"` se marcaron como nulos.
   - `reg_date`: se marcaron como nulas ~40 fechas de 2026 (año no transcurrido dentro del alcance del dataset `usage`, que cubre 2024).
   - `duration`: se corrigieron 16 registros de tipo `text` que tenían erróneamente una duración de 120 (los mensajes de texto no deberían tener duración).
   - Los nulos en `duration` y `length` se evaluaron como **MAR** (dependen de la columna `type`) y se dejaron como nulos de forma justificada.
4. **Estadísticas de uso por usuario** — Agregación de `usage.csv` por `user_id` para construir `user_profile`, con métricas como `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`, combinadas con la información de `users`.
5. **Visualización de distribuciones y detección de outliers** — Histogramas por plan (`Básico` vs `Premium`) para edad y variables de uso, y boxplots con el **método IQR** para identificar valores extremos.
6. **Segmentación de clientes**:
   - Por **nivel de uso**: Bajo uso, Uso medio, Alto uso (según cantidad de llamadas y mensajes).
   - Por **edad**: Joven (<30), Adulto (30–60), Adulto Mayor (>60).
7. **Insight ejecutivo** — Síntesis de hallazgos y recomendaciones comerciales para stakeholders de ConnectaTel.

## 📊 Hallazgos principales

- La mayoría de los clientes (~50.5%) son **Adultos** (30–60 años), seguidos por Adultos Mayores (~30.6%) y Jóvenes (~19%).
- El **73.6%** de los clientes se ubica en el segmento de **Uso medio**; solo el 6.8% son de Alto uso.
- El comportamiento de consumo de minutos es similar entre planes Básico y Premium, lo que sugiere que el valor percibido del plan Premium podría no estar ligado a los minutos incluidos.
- Se recomienda diseñar un plan intermedio entre Básico y Premium y dirigir campañas de fidelización al segmento de Adultos con plan Básico.

Link al repositorio público del proyecto: `[]`
