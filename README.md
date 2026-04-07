# Proyecto de Investigación Big Data
## Conectividad a Internet y Resultados ICFES en Municipios de Colombia
## Autores: Ana Sofía Arboleda, Nicolás Torres, Alejandro Molina, Miguel Espinosa

**Pontificia Universidad Javeriana**  
Facultad de Ingeniería — Departamento de Ingeniería de Sistemas  
Procesamiento Alto Volumen de Datos — John Corredor
Entrega 1: Entendimiento del Negocio y de los Datos

---

## Tabla de Contenidos

- [Descripción del Proyecto](#descripción-del-proyecto)
- [Pregunta de Negocio Central](#pregunta-de-negocio-central)
- [Municipios de Estudio](#municipios-de-estudio)
- [Justificación de la Selección](#justificación-de-la-selección)
- [Datasets Utilizados](#datasets-utilizados)
- [Estructura del Repositorio](#estructura-del-repositorio)
- [Descripción de Scripts](#descripción-de-scripts)
- [Instrucciones de Ejecución](#instrucciones-de-ejecución)
- [Metodología CRISP-DM](#metodología-crisp-dm)
- [Estructura de Entregas](#estructura-de-entregas)
- [Equipo de Trabajo](#equipo-de-trabajo)
- [Referencias](#referencias)

---

## Descripción del Proyecto

Este proyecto analítico fue desarrollado en el rol de consultores del **Ministerio de Educación de Colombia**, con el propósito de identificar y cuantificar la relación entre la **cobertura del servicio de internet por municipio** y los **resultados de las Pruebas Saber 11 (ICFES)**, y a partir de ello generar un plan de acción basado en evidencia para mejorar dichos indicadores en los municipios de mayor rezago.

El proyecto sigue la metodología **CRISP-DM** (*Cross-Industry Standard Process for Data Mining*) e implementa procesamiento distribuido con **Apache Spark** y su biblioteca de aprendizaje de máquina **MLlib**, sobre un clúster de alto rendimiento .

---

## Pregunta de Negocio Central

> ¿Cuál es la relación entre el acceso a internet por municipio y el rendimiento académico en las Pruebas Saber 11, y qué variables socioeconómicas intermedian o amplifican esa relación en municipios con condiciones estructurales de vulnerabilidad?

---

## Municipios de Estudio

Se seleccionaron cuatro municipios colombianos que representan realidades socioeconómicas, geográficas y de conectividad deliberadamente contrastadas. Esta selección intencionada maximiza el poder explicativo del análisis y la relevancia de las recomendaciones de política pública.

| Municipio | Departamento | Perfil de conectividad | Población estimada (2023) |
|-----------|:------------:|:---------------------:|:-------------------------:|
| Bogotá D.C. | Cundinamarca | Alta | 8.380.801 |
| Buenaventura | Valle del Cauca | Baja | 440.882 |
| Riohacha | La Guajira | Baja | 315.023 |
| Quibdó | Chocó | Muy baja | 126.495 |

---

## Justificación de la Selección

La elección de estos cuatro municipios no es arbitraria. Cada uno aporta una dimensión analítica específica al estudio.

**Bogotá D.C.** funciona como referente nacional y caso de control. Cuenta con la mayor infraestructura digital del país, los mejores indicadores educativos agregados y la mayor densidad de instituciones de educación media. Su inclusión permite establecer una línea de comparación clara frente a los municipios rezagados.

**Buenaventura** representa una paradoja estructural de alta relevancia para el análisis: es el puerto marítimo más importante de Colombia sobre el Pacífico y uno de los nodos logísticos más estratégicos del país, pero al mismo tiempo registra índices de pobreza multidimensional superiores al 40%, cobertura de internet significativamente por debajo del promedio nacional y resultados ICFES consistentemente bajos. Este contraste entre su importancia económica y su rezago social lo convierte en un caso de estudio prioritario.

**Riohacha** incorpora al análisis la dimensión étnica y cultural. Como capital del departamento de La Guajira, concentra una proporción significativa de población Wayuu y enfrenta condiciones de pobreza extrema, deficiencias históricas en infraestructura de servicios públicos y una de las coberturas de internet más bajas entre las capitales departamentales del país. Su inclusión permite examinar si la brecha digital reproduce o amplifica desigualdades educativas preexistentes en contextos de diversidad étnica.

**Quibdó** representa el caso de mayor rezago dentro del conjunto seleccionado. Es la capital departamental con menor cobertura de internet fijo en Colombia, enfrenta las precipitaciones más altas del país —lo que impone costos adicionales sobre la infraestructura de telecomunicaciones—, y registra los puntajes ICFES más bajos entre las ciudades analizadas. Su inclusión es fundamental para anclar el análisis en el extremo inferior de la distribución y dar visibilidad a una realidad frecuentemente subrepresentada en estudios de política educativa.

En conjunto, los cuatro municipios conforman un gradiente que va desde la alta conectividad y alto rendimiento educativo (Bogotá D.C.) hasta la baja conectividad y bajo rendimiento (Quibdó), pasando por dos casos intermedios con características estructurales distintas (Buenaventura y Riohacha). Este gradiente es el que otorga fuerza narrativa y analítica al proyecto.

---

## Datasets Utilizados

| Dataset | Fuente oficial | Descripción | Período cubierto |
|---------|:--------------:|-------------|:----------------:|
| Servicio de internet por municipio | MinTIC | Accesos fijos y móviles por municipio, desagregados por tecnología | 2015–2023 |
| Resultados ICFES Saber 11 | ICFES — Datos Abiertos | Puntajes globales y por área para los municipios de estudio | 2015–2023 |
| Pobreza por municipio | DNP / DANE | Índice de Pobreza Multidimensional (IPM) municipal | 2015–2022 |
| Índice Municipal de Gestión de Riesgo | UNGRD | Indicador de vulnerabilidad territorial ante riesgos naturales | 2015–2023 |
| Niveles de educación (fuente A) | MEN | Cobertura neta por nivel educativo (preescolar, primaria, secundaria, media) | 2015–2022 |
| Niveles de educación (fuente B) | DANE | Población por nivel educativo alcanzado | 2018–2022 |
| Población municipal | DANE (web scraping) | Proyecciones de población por municipio, usadas para cálculos per cápita | 2018–2023 |
| Datos climáticos | Open-Meteo API | Temperatura máxima diaria y precipitación acumulada por municipio | 2018–2023 |

---

## Estructura del Repositorio

```
BigData_PUJ/
|
+-- data/
|   |-- internet_municipio.csv          Dataset MinTIC descargado
|   |-- icfes_resultados.csv            Dataset ICFES descargado
|   |-- pobreza_municipio.csv           Dataset DNP descargado
|   |-- indice_gestion_riesgo.csv       Dataset UNGRD descargado
|   |-- niveles_educacion_a.csv         Dataset MEN descargado
|   |-- niveles_educacion_b.csv         Dataset DANE descargado
|   +-- datos_climaticos.csv            Generado por script 04 via API
|
+-- notebooks/
|   |-- 01_carga_descripcion.ipynb      Carga y descripción de datos
|   |-- 02_eda_exploracion.ipynb        Análisis exploratorio de datos
|   |-- 03_calidad_limpieza.ipynb       Calidad, filtros y transformaciones
|   |-- 04_bonos.ipynb                  Web scraping y API climática
|   |-- 05_modelado.ipynb               Modelos ML con MLlib (Entrega 2)
|   +-- 06_resultados_negocio.ipynb     Respuestas a preguntas (Entrega 2)
|
+-- scripts/
|   |-- 01_carga_datos.py               Inicialización de Spark y carga
|   |-- 02_eda_visualizaciones.py       10 análisis EDA con visualizaciones
|   |-- 03_calidad_transformaciones.py  Calidad de datos y pipeline de limpieza
|   +-- 04_bonos_scraping_clima.py      Web scraping (Bono A) y API (Bono B)
|
+-- graficas/
|   |-- 02_distribucion_internet.png
|   |-- 03_violin_icfes_municipio.png
|   |-- 04_scatter_internet_icfes.png
|   |-- 05_heatmap_correlacion.png
|   |-- 06_barras_comparativa_municipios.png
|   |-- 07_serie_tiempo_internet.png
|   |-- 08_bubble_pobreza_icfes.png
|   |-- 09_boxplot_zona_icfes.png
|   |-- 10_lollipop_ranking_icfes.png
|   |-- 11_heatmap_valores_faltantes.png
|   |-- 12_web_scraping_poblacion.png
|   +-- 13_datos_climaticos_api.png
|
+-- docs/
|   |-- entrega1_bigdata_puj.pdf        Documento Entrega 1 (PDF final)
|   |-- entrega2_bigdata_puj.pdf        Documento Entrega 2 (PDF final)
|   +-- plantilla_bigdata_entrega1.tex  Plantilla LaTeX del proyecto
|
+-- references/
|   +-- referencias.bib                 Bibliografía exportada desde Zotero
|
+-- README.md                           Este archivo
+-- requirements.txt                    Dependencias Python del proyecto
+-- .gitignore                          Archivos y carpetas excluidos del repo
```

---

## Descripción de Scripts

### `scripts/01_carga_datos.py`

Inicializa la SparkSession con configuración optimizada para el clúster del proyecto y carga todos los datasets en DataFrames de Spark. Registra cada DataFrame como vista temporal SQL para consultas directas.

| Componente | Descripción |
|------------|-------------|
| SparkSession | Configurada con 4 GB de memoria para el driver y 8 particiones de shuffle |
| `cargar_dataset(ruta, nombre, sep)` | Carga un CSV con inferencia de esquema, manejo de encoding UTF-8 y logging de métricas de filas y columnas |
| Vistas temporales | Registra `internet`, `icfes`, `pobreza`, `riesgo`, `educacion_a`, `educacion_b` para uso directo con `spark.sql()` |

Salida: DataFrames disponibles en sesión, listos para ser consumidos por los scripts siguientes.

---

### `scripts/02_eda_visualizaciones.py`

Genera los diez análisis exploratorios del EDA con gráficas exportadas en PNG a 300 DPI, listas para insertar en el documento LaTeX. Utiliza datos sintéticos representativos calibrados sobre los cuatro municipios del estudio como demostración; en producción se reemplazan por los DataFrames reales convertidos con `.toPandas()`.

| Numero | Tipo de gráfica | Variables involucradas |
|:------:|-----------------|------------------------|
| 1 | Tabla de estadísticos descriptivos | Todas las variables numéricas |
| 2 | Histograma con estimación KDE | `internet_per_cap` |
| 3 | Diagrama de violín | `puntaje_icfes` por municipio |
| 4 | Diagrama de dispersión con tendencia OLS | `internet_per_cap` vs `puntaje_icfes` |
| 5 | Mapa de calor de correlaciones | Todas las variables numéricas |
| 6 | Barras horizontales comparativas | Internet, ICFES y pobreza por municipio |
| 7 | Serie de tiempo | `internet_per_cap` por año y municipio |
| 8 | Diagrama de burbuja | `indice_pobreza` vs `puntaje_icfes` |
| 9 | Box plot con barras de error estándar | `puntaje_icfes` por zona urbana y rural |
| 10 | Lollipop chart | Ranking de municipios por `puntaje_icfes` |

Todas las gráficas siguen la paleta de colores institucional de la Pontificia Universidad Javeriana, con `#00539C` como color primario.

---

### `scripts/03_calidad_transformaciones.py`

Evalúa la calidad de los datos y ejecuta el pipeline de limpieza y transformación sobre los DataFrames de Spark. Diseñado como conjunto de funciones modulares e independientes que pueden aplicarse sobre cualquier DataFrame del proyecto.

| Función | Descripción |
|---------|-------------|
| `reporte_nulos(df, nombre)` | Calcula el porcentaje de valores nulos por columna y genera una recomendación de tratamiento automática según el umbral de faltantes detectado |
| `visualizar_nulos(reportes)` | Genera un mapa de calor del porcentaje de valores faltantes cruzando variables y datasets |
| `aplicar_filtros(df, nombre)` | Aplica tres filtros: rango temporal 2015–2023, eliminación de valores negativos en variables numéricas clave y selección de los cuatro municipios del estudio |
| `transformacion_internet_per_capita()` | Calcula accesos a internet por cada 1.000 habitantes (transformación T1) |
| `normalizacion_minmax()` | Normalización Min-Max al intervalo [0,1] usando `MinMaxScaler` de MLlib (transformación T2) |
| `imputacion_por_mediana()` | Imputa valores nulos por la mediana calculada con `approxQuantile` de Spark (transformación T3) |
| `estandarizar_nombres_municipio()` | Convierte nombres de municipio a mayúsculas y elimina espacios redundantes para garantizar joins correctos entre datasets (transformación T4) |
| `codificar_zona()` | Codifica la variable zona (Urbano/Rural) como índice numérico mediante `StringIndexer` de MLlib (transformación T5) |
| `pipeline_transformaciones(df)` | Orquesta la aplicación secuencial de todas las transformaciones anteriores sobre un DataFrame dado |

---

### `scripts/04_bonos_scraping_clima.py`

Implementa los dos bonos opcionales de la Entrega 1.

**Bono A — Web Scraping de Población:**

| Función | Descripción |
|---------|-------------|
| `scraping_poblacion(url)` | Realiza la solicitud HTTP con headers de navegador, parsea la tabla HTML con BeautifulSoup y retorna un DataFrame de pandas |
| `limpiar_df_poblacion(df)` | Estandariza nombres de columnas, elimina encabezados anidados y convierte campos numéricos con formato de separador de miles |
| `graficar_poblacion(df, ...)` | Genera barras horizontales y gráfica de participación porcentual con los datos scrapeados; incluye respaldo automático con datos DANE 2023 si la solicitud HTTP falla |

**Bono B — API Climática Open-Meteo:**

| Función | Descripción |
|---------|-------------|
| `consultar_clima(municipio, lat, lon)` | Consulta la API histórica de Open-Meteo sin clave de API para temperatura máxima diaria y precipitación acumulada |
| `obtener_todos_los_climas()` | Itera los cuatro municipios del estudio con pausa configurable entre solicitudes para respetar los límites de la API |
| `graficar_clima(df_clima)` | Genera cuatro gráficas: temperatura mensual por municipio, precipitación acumulada anual, distribución de temperaturas (box plot) y mapa de calor temperatura-mes |

Las coordenadas geográficas configuradas corresponden a los cuatro municipios del estudio: Bogotá D.C. (4.711°N, 74.072°O), Buenaventura (3.881°N, 77.031°O), Riohacha (11.544°N, 72.907°O) y Quibdó (5.694°N, 76.658°O).

---

## Instrucciones de Ejecución

### Opción 1 — Databricks Community Edition (recomendada)

Databricks Community Edition provee un clúster Apache Spark preconfigurado de forma gratuita, sin necesidad de instalar ni configurar infraestructura local.

```
1. Crear cuenta en https://community.cloud.databricks.com
2. Crear un clúster con Runtime 13.x ML (incluye Spark 3.4 y Python 3.10)
3. Importar los notebooks desde la carpeta /notebooks/ del repositorio
4. Subir los archivos de datos a DBFS en la ruta /FileStore/bigdata_puj/data/
5. Ejecutar los notebooks en orden: 01, 02, 03, 04
```

### Opción 2 — Google Colab

```python
# Celda 1: instalar dependencias
!pip install pyspark findspark pandas numpy matplotlib seaborn requests beautifulsoup4

# Celda 2: inicializar Spark
import findspark
findspark.init()

# Celda 3: ejecutar el script de carga
exec(open("scripts/01_carga_datos.py").read())
```

### Opción 3 — Clúster local con máquinas virtuales

```bash
# Instalar dependencias
pip install -r requirements.txt

# Ejecutar en orden
spark-submit scripts/01_carga_datos.py
python        scripts/02_eda_visualizaciones.py
python        scripts/03_calidad_transformaciones.py
python        scripts/04_bonos_scraping_clima.py
```

---

## Metodología CRISP-DM

El proyecto sigue las seis fases de la metodología CRISP-DM, distribuidas entre las dos entregas:

```
Entrega 1                                  Entrega 2
------------------------------------------  ------------------------------------------
Fase 1: Entendimiento del negocio           Fase 3: Preparación de datos
Fase 2: Entendimiento de los datos          Fase 4: Modelado con MLlib y Spark
                                            Fase 5: Evaluación de modelos
                                            Fase 6: Despliegue (plan de acción)
```

La naturaleza iterativa de CRISP-DM se refleja en la posibilidad de revisar la selección de datos y el planteamiento de preguntas entre entregas, conforme el análisis exploratorio revele características inesperadas en los datos.

---

## Estructura de Entregas

### Entrega 1 — Entendimiento del negocio y de los datos

| Seccion | Contenido | Responsable |
|:-------:|-----------|:-----------:|
| 1 | Entendimiento del negocio e indicadores macroeconómicos | Persona 1 |
| 2 | Selección y justificación de datasets | Persona 1 |
| 3 | Carga en Spark y descripción de atributos | Persona 2 |
| 4 | Exploración de datos — 10 análisis visuales | Persona 3 |
| 5 | Reporte de calidad de datos | Persona 3 |
| 6 | Planteamiento de 8 preguntas de negocio | Persona 4 |
| 7 | Filtros, limpieza y transformaciones iniciales | Persona 2 |
| Bono A | Web scraping de datos de población | Persona 4 |
| Bono B | Extracción de datos climáticos via API | Persona 2 |

Calificación: Documento (60%) + Sustentación oral 15 minutos (40%)

### Entrega 2 — Preparación de datos, modelado y resultados

| Seccion | Contenido |
|:-------:|-----------|
| 1 | Filtros y transformaciones finales (mínimo 2 filtros y 3 transformaciones) |
| 2 | Respuestas a las 8 preguntas de negocio con tablas y visualizaciones |
| 3 | Selección justificada de 1 técnica supervisada y 1 no supervisada |
| 4 | Preparación de datos para modelado: correlaciones, normalización, selección de variables |
| 5 | Aplicación de modelos con MLlib sobre el clúster Spark |
| 6 | Métricas de evaluación con variación de hiperparámetros |
| Bono | Construcción de un modelo de aprendizaje profundo (red neuronal) |

Calificación: Documento (60%) + Sustentación oral 15 minutos (40%)

### Bonos sobre la calificación final

| Bono | Requisito | Puntos adicionales |
|:----:|-----------|:-----------------:|
| Bibliografía | Referencias gestionadas con Zotero o Mendeley | +0.1 |
| README | Repositorio con README completo y documentado | +0.05 |

---

## Equipo de Trabajo

| Nombre completo | Correo institucional | Responsabilidad principal |
|-----------------|---------------------|--------------------------|
| [Nombre Apellido 1] | correo1@javeriana.edu.co | Entendimiento de negocio y selección de datos |
| [Nombre Apellido 2] | correo2@javeriana.edu.co | Infraestructura Spark, carga y transformaciones |
| [Nombre Apellido 3] | correo3@javeriana.edu.co | Análisis exploratorio y calidad de datos |
| [Nombre Apellido 4] | correo4@javeriana.edu.co | Preguntas de negocio y bonos |

---

## Referencias

La bibliografía completa se gestiona con **Zotero** y se exporta en formato BibLaTeX al archivo [`references/referencias.bib`](references/referencias.bib), vinculado directamente con la plantilla LaTeX mediante el paquete `biblatex` con backend `biber`.

Fuentes institucionales principales:

- Ministerio de Tecnologías de la Información y las Comunicaciones (MinTIC). Boletines de conectividad. https://colombiatic.mintic.gov.co
- Instituto Colombiano para la Evaluación de la Educación (ICFES). Datos abiertos Saber 11. https://www.icfes.gov.co/data-icfes
- Departamento Administrativo Nacional de Estadística (DANE). Proyecciones de población municipal 2018–2035. https://www.dane.gov.co
- Departamento Nacional de Planeación (DNP). Índice de Pobreza Multidimensional por municipio. https://www.dnp.gov.co
- Unidad Nacional para la Gestión del Riesgo de Desastres (UNGRD). Índice Municipal de Gestión del Riesgo. https://www.ungrd.gov.co
- Open-Meteo. Historical Weather API. https://open-meteo.com

---

*Pontificia Universidad Javeriana — Facultad de Ingeniería — 2026*  
*Procesamiento Alto Volumen de Datos 