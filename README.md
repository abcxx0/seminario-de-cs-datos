# seminario-de-cs-datos
1.  Selección y Depuración de Datos

En esta sección, se describe el proceso de carga y exploración inicial del conjunto de datos sobre reseñas de vinos. El objetivo es garantizar que el archivo se pueda manipular adecuadamente y proceder a una limpieza preliminar que permita su análisis.
- Técnica de Depuración 
#Carga de Archivo Reseña de vinos
from google.colab import drive
drive.mount('/content/drive')

import pandas as pd

file_path = '/content/drive/MyDrive/Siglo21/Entregable 2 - Depurado.xlsx'
df = pd.read_excel(file_path)

Este código permite cargar el conjunto de datos en el entorno de trabajo, facilitando la exploración y el análisis de las reseñas de vinos.


Exploración de datos

Después de cargar los datos, es fundamental entender la estructura general del archivo y realizar un análisis preliminar. Esto incluye revisar las primeras filas del archivo, verificar los tipos de datos y el número de entradas no nulas.

# Ver las primeras filas del DataFrame
print(df.head())

# Información general sobre las columnas y tipos de datos
print(df.info())

# Descripción estadística de las columnas numéricas
print(df.describe())


- Identificación de valores nulos
Para asegurar un análisis limpio, es importante identificar cuántos valores nulos hay en cada columna y decidir cómo manejarlos.
# Identificar valores nulos en cada columna
print(df.isnull().sum())

Tratamiento de valores nulos 

El tratamiento de los valores nulos es un paso esencial en la depuración de datos. Los valores ausentes pueden afectar la calidad de los análisis y llevar a resultados poco precisos. En este paso, identificamos los valores nulos y aplicamos estrategias de imputación adecuadas para diferentes columnas.

# Identificar y tratar valores nulos en columnas clave
print("\nValores nulos antes de la limpieza:\n", df.isnull().sum())
# Rellenar valores nulos en 'price' con la mediana de la columna
df['price'] = df['price'].fillna(df['price'].median())
# Rellenar valores nulos en 'country' con “Unknown"
df['country'] = df['country'].fillna("Unknown")

# Rellenar valores nulos en 'province' con "Unknown"
df['province'] = df[‘province'].fillna("Unknown")

# Rellenar valores nulos en 'region_1' con “Unknown"
df['region_1'] = df['region_1'].fillna("Unknown")

# Rellenar valores nulos en 'region_2' con “Unknown"
df['region_2'] = df['region_2'].fillna("Unknown")
Explicación:
Rellenar valores numéricos con la mediana es una práctica común para preservar la distribución de los datos sin introducir sesgos significativos.
Rellenar valores categóricos con "Unknown" permite conservar la mayor cantidad de datos posible sin tener que eliminar registros completos, lo que ayuda a mantener el tamaño de la muestra para futuros análisis.
Después de ejecutar este código, el DataFrame estará libre de valores nulos en las columnas clave, facilitando un análisis más robusto y confiable.

- Conversión de tipos de datos en columnas numéricas
El propósito de convertir los tipos de datos a formatos numéricos es garantizar que las columnas se traten de manera correcta durante los análisis estadísticos y matemáticos.
# Convertir tipos de datos en columnas numéricas

df['points'] = pd.to_numeric(df['points'], errors='coerce') # Asegurar que 'points' sea numérico
df['price'] = pd.to_numeric(df['price'], errors='coerce') # Asegurar que 'price' sea numérico

Eliminación de columnas no relevantes 

# Lista de columnas que se quieren eliminar

columns_to_drop = ['taster_name', 'taster_twitter_handle', 'title']
# Filtrar la lista para incluir solo las columnas que existen en el DataFrame

existing_columns_to_drop = [col for col in columns_to_drop if col in df.columns]
# Eliminar las columnas existentes del DataFrame

df = df.drop(columns=existing_columns_to_drop)

El DataFrame se actualizará sin las columnas especificadas que existían, dejando solo las columnas relevantes para el análisis. Este paso ayuda a simplificar el conjunto de datos y a centrar los recursos de procesamiento en la información importante.

Eliminación de registros duplicados

# Eliminar registros duplicados
df = df.drop_duplicates()

Elimina las filas duplicadas en el DataFrame para evitar sesgos en los análisis. Esto garantiza que cada registro sea único y representa un caso distinto.

- Guardar el archivo CSV en Google Drive

import os

# Crear la carpeta en Google Drive si no existe

os.makedirs("/content/drive/MyDrive/siglo21", exist_ok=True)

# Guardar el DataFrame en la carpeta de trabajo local y en Google Drive

df.to_csv("siglo21/winemag-data-130k-cleaned.csv", index=False)
print("Datos seleccionados y depurados correctamente.")
df.to_csv("/content/drive/MyDrive/siglo21/winemag-data-130k-cleaned.csv", index=False)


El código se encarga de crear el directorio en Google Drive y luego guardar el DataFrame limpio en un archivo CSV en dicha ubicación. Este paso es útil para garantizar que el archivo esté accesible fuera del entorno de Colab y se almacene de manera segura en el espacio de Google Drive.


Justificación de las Técnicas Utilizadas:

Las técnicas de depuración de datos utilizadas en este proyecto tienen como objetivo maximizar la calidad del conjunto de datos para análisis posteriores. La imputación de valores nulos con la mediana en variables numéricas y con "Unknown" en variables categóricas permite preservar la mayor cantidad de datos sin sesgar las distribuciones. La conversión de tipos de datos y la eliminación de columnas no relevantes simplifican la estructura del DataFrame, haciendo el análisis más eficiente y preciso. La eliminación de duplicados evita que los resultados se vean sesgados por entradas repetidas. Finalmente, guardar el archivo tanto localmente como en Google Drive asegura un respaldo y accesibilidad a los datos limpios para futuros análisis.


2. Contextualización y preguntas clave

Contextualización del Conjunto de Datos:
El conjunto de datos consiste en reseñas de vinos, con información sobre la calificación, el precio, la región de producción, el país y descripciones proporcionadas por expertos. Comprender el contexto de estos datos es crucial para establecer preguntas de análisis que ayuden a extraer información significativa. Las reseñas de vinos pueden proporcionar insights sobre tendencias de calidad, accesibilidad de precios y regiones que destacan en la producción de vinos de alta calidad.
Preguntas Claves
1. ¿Cuáles son los países con mayor cantidad de reseñas de vinos?
Esta pregunta es fundamental para entender la representación de los datos en términos de origen. Ayuda a identificar qué países son los más prominentes en el mercado de reseñas de vinos y en el conjunto de datos, estableciendo una base para comparaciones más detalladas.
2. ¿Existe una relación entre el precio y la puntuación de los vinos?
Esta es una pregunta clave para evaluar si un precio más alto está asociado con una mejor calidad. Es útil para consumidores, productores y distribuidores que buscan entender la correlación entre estos dos factores.
3. ¿Cuáles son las variedades de vino con mejores puntuaciones promedio?
Conocer qué variedades de uva reciben mejores puntuaciones permite destacar tendencias en preferencias de calidad, lo cual es valioso para la industria vitivinícola y para los consumidores informados.
4. ¿Cuál es la distribución de puntuaciones de los vinos en el dataset?
Ver la distribución de las puntuaciones ayuda a comprender la calidad general de los vinos representados en el conjunto de datos y permite identificar si hay sesgos hacia puntuaciones más altas o bajas.
5. ¿Cómo se distribuye la relación calidad-precio entre las principales variedades de vino?
Analizar la relación calidad-precio (puntuación/precio) indica cuánto valor obtiene el consumidor por cada unidad de dinero invertido. Al comparar esta relación entre las variedades, podemos identificar cuáles ofrecen el mejor valor y cuáles tienen un precio elevado sin justificarlo con una puntuación alta. Esta información es útil tanto para los consumidores, que buscan obtener el mejor valor, como para los productores, que pueden ajustar sus precios para ser más competitivos.

Creación de visualizaciones 
Se desarrollaron visualizaciones específicas utilizando Python con las bibliotecas matplotlib, seaborn y plotly. Cada visualización está diseñada para proporcionar insights claros y significativos sobre los diferentes aspectos del dataset de vinos.
Las visualizaciones incluyen:
Gráfico de barras → Países con más reseñas
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

import seaborn as sns
import matplotlib.pyplot as plt
# 1. Países con mayor cantidad de reseñas
# Análisis de los países con mayor representación en el dataset de reseñas
top_countries = df['country'].value_counts().head(10)

# Configuración del gráfico
plt.figure(figsize=(12, 8))

sns.barplot(y=top_countries.index, x=top_countries.values, palette="YlOrRd")

# Agregar los valores sobre las barras
for index, value in enumerate(top_countries.values):
	plt.text(value + 200, index, f'{value}', va='center', color='black', fontweight='bold', fontsize=12)

# Títulos y etiquetas 
plt.title("Países con Mayor Cantidad de Reseñas de Vinos", fontsize=16, fontweight='bold', color='black')
plt.xlabel("Cantidad de Reseñas", fontsize=14, fontweight='bold')
plt.ylabel("País", fontsize=14, fontweight='bold')

# Cambiar el fondo del gráfico
sns.set_style("whitegrid")
plt.gcf().set_facecolor("whitesmoke")

# Ocultar la leyenda ya que no es necesaria en este caso
plt.legend([], [], frameon=False)
# Ajuste de márgenes para no cortar las etiquetas
plt.tight_layout()
# Mostrar el gráfico
plt.show()


El código crea este gráfico de barras horizontal, donde se visualizan las cantidades de reseñas de cada país, añade los valores numéricos sobre las barras y ajusta los títulos, etiquetas y el fondo para mejorar la claridad visual. Además, oculta la leyenda y ajusta los márgenes para asegurar una presentación adecuada.
Análisis General de la Distribución Global de Reseñas
El panorama mundial de reseñas vinícolas revela una distribución notablemente asimétrica, donde Estados Unidos emerge como el líder indiscutible con 54,504 reseñas, más del doble que su seguidor más cercano. Este dominio estadounidense en la cantidad de reseñas refleja no solo un mercado maduro, sino también una cultura vinícola activa y participativa. Francia e Italia, con 22,093 y 19,540 reseñas respectivamente, conforman un segundo tier de importancia, representando los mercados tradicionales europeos con una sólida presencia crítica. La presencia de países del Nuevo Mundo vinícola, como Chile (4,472 reseñas) y Argentina (3,800 reseñas), junto con Australia (2,329 reseñas), demuestra la globalización del mercado del vino y la creciente importancia de estas regiones en el panorama internacional.

Perspectiva para Productores Vinícolas
Oportunidades Estratégicas en el Mercado Global
El dominio abrumador de Estados Unidos en términos de reseñas presenta una oportunidad sin precedentes para los productores vinícolas. Este mercado no solo ofrece el mayor volumen de retroalimentación crítica, sino que también sugiere una base de consumidores altamente comprometida y educada en términos vinícolas. La significativa presencia de reseñas en mercados tradicionales como Francia e Italia indica que estos países mantienen su relevancia como referentes de calidad y tradición vinícola, aspectos que los productores deben considerar al desarrollar sus productos y estrategias de mercado.

Recomendaciones para el Desarrollo Productivo
Los productores deberían considerar el desarrollo de productos que respondan a las preferencias específicas de los mercados más activos en términos de reseñas. La considerable diferencia entre los primeros tres países (Estados Unidos, Francia e Italia) y el resto sugiere la necesidad de estrategias diferenciadas: mientras que en los mercados principales se requiere un enfoque en la diferenciación y la calidad premium para destacar entre numerosas reseñas, en mercados con menor cantidad de reseñas como Austria (3,345) o Alemania (2,165), podría ser más efectivo enfocarse en nichos específicos y construcción de marca.

Análisis para Comerciantes de Vino:
Oportunidades de Mercado Identificadas
La distribución de reseñas revela patrones claros de actividad comercial y engagement del consumidor. El volumen extraordinario de reseñas en Estados Unidos señala un mercado no solo grande sino también altamente participativo, donde los consumidores buscan activamente información y comparten sus experiencias. Los mercados europeos tradicionales, representados por Francia e Italia, muestran una actividad crítica sustancial que sugiere un consumidor educado y exigente. La presencia significativa pero menor de países como España (6,645 reseñas) y Portugal (5,691 reseñas) indica mercados maduros con potencial de crecimiento en términos de engagement crítico.

Estrategias Comerciales Recomendadas
Los comerciantes deberían desarrollar estrategias que aprovechen la intensidad de la actividad crítica en cada mercado. En Estados Unidos, donde el volumen de reseñas es extraordinariamente alto, es fundamental desarrollar una presencia digital robusta y mantener relaciones activas con críticos y evaluadores. En mercados con menor cantidad de reseñas pero tradición vinícola significativa, como Austria y Alemania, la estrategia debería enfocarse en construir credibilidad y educar al consumidor sobre las características distintivas de los productos.



Conclusión y Perspectivas Futuras
La distribución global de reseñas de vinos revela un mercado dinámico y en evolución, donde la tradición se encuentra con la modernidad. Los datos sugieren que el futuro del mercado vinícola estará determinado por la capacidad de productores y comerciantes para adaptarse a las preferencias y comportamientos de los consumidores en diferentes regiones, siempre manteniendo un equilibrio entre la innovación y el respeto por la tradición vinícola. La notable diferencia en el volumen de reseñas entre países sugiere que existe un amplio espacio para el crecimiento y desarrollo en muchos mercados, presentando oportunidades significativas tanto para productores como para comerciantes que sepan interpretar y aprovechar estas tendencias.

2. Gráfico de dispersión → Relación precio-puntuación
# 2. Relación entre precio y puntuación
# Configuración del gráfico
plt.figure(figsize=(12, 8))
sns.set_style("whitegrid")
plt.gcf().set_facecolor("whitesmoke")  # Fondo suave

# Crear el gráfico de dispersión con gradiente de colores
scatter = plt.scatter(
    x=df['price'], y=df['points'], 
    c=df['price'], cmap="YlOrRd", alpha=0.6
)
# Agregar barra de colores para el gradiente
cbar = plt.colorbar(scatter)
cbar.set_label("Precio (USD)", fontsize=12, fontweight='bold')

# Títulos y etiquetas
plt.title("Relación entre Precio y Puntuación de los Vinos", fontsize=16, fontweight='bold', color='black')
plt.xlabel("Precio (USD)", fontsize=14, fontweight='bold')
plt.ylabel("Puntuación", fontsize=14, fontweight=‘bold’)

# Ajustar diseño y mostrar gráfico
plt.tight_layout()
plt.show()

Este código genera un gráfico de dispersión para analizar la relación entre el precio (`df['price']`) y la puntuación (`df['points']`)de los vinos. Utiliza un gradiente de colores (amarillo a rojo) para representar precios bajos a altos, con transparencia en los puntos para destacar áreas densas. También se incluyen títulos, etiquetas y una barra de color para interpretar mejor la visualización.
El gráfico muestra que los vinos más caros (puntos rojos y naranjas) tienden a tener puntuaciones más altas. Sin embargo, hay excepciones: algunos vinos económicos obtienen buenas puntuaciones y viceversa. Esto ilustra una relación general positiva, pero no determinante, entre precio y calidad en los vinos.

Análisis de la Relación Precio-Puntuación en Vinos y su Relevancia para el Sector
Descripción del Análisis
El gráfico de dispersión presenta la relación entre el precio (en USD) y la puntuación de los vinos, con un rango de precios que se extiende desde 0 hasta 30,000 USD y puntuaciones entre 80 y 100 puntos. La intensidad del color (de amarillo claro a rojo oscuro) indica la concentración de vinos en cada nivel de precio, mostrando una distribución interesante de la relación calidad-precio en el mercado.

Perspectiva para Productores
Para los productores de vino, este gráfico revela patrones significativos que pueden informar sus estrategias de producción. La mayor concentración de puntos se observa en el rango de precios más bajo (0-5,000 USD), donde las puntuaciones varían considerablemente desde 80 hasta 100 puntos. Este patrón sugiere que es posible alcanzar alta calidad incluso en segmentos de precio más accesibles.
Es particularmente notable que existen vinos con puntuaciones excepcionales (95-100) en el rango de precio más económico, lo que indica que la excelencia en la producción no está necesariamente vinculada a costos de producción extremadamente altos. Para los productores, esto significa que pueden enfocarse en optimizar sus procesos de vinificación para maximizar la calidad sin necesidad de incurrir en costos extraordinarios.

Implicaciones para Comerciantes
Desde la perspectiva comercial, la visualización ofrece información valiosa para la estrategia de mercado. La presencia de vinos altamente puntuados (sobre 95 puntos) en diversos rangos de precio permite desarrollar portfolios diversificados. Los comerciantes pueden identificar "joyas ocultas" - vinos de alta puntuación a precios moderados - que representan excelentes oportunidades de valor para sus clientes.
El gráfico muestra también que los vinos más caros (sobre 15,000 USD, representados en tonos rojizos) tienden a mantener puntuaciones consistentemente altas, aunque no necesariamente las más altas del mercado. Esto sugiere que el segmento de lujo se basa en factores adicionales más allá de la puntuación pura, como la rareza o el prestigio de la bodega.

Conclusiones y Recomendaciones Estratégicas
La visualización revela varias conclusiones clave para la industria vitivinícola:
Para Productores:
- La excelencia en calidad es alcanzable en todos los niveles de precio, como lo demuestran los puntos amarillos con altas puntuaciones en rangos de precio más bajos
- El enfoque debe estar en la optimización de procesos de producción, ya que el precio no garantiza automáticamente puntuaciones más altas
- Existe una oportunidad significativa en el segmento de precio medio-bajo para producir vinos de alta calidad que destaquen por su relación calidad-precio
Para Comerciantes:
- El mercado ofrece oportunidades para construir un portfolio diversificado que incluya vinos excepcionales en múltiples niveles de precio
- Los vinos de precio extremadamente alto (sobre 20,000 USD) pueden comercializarse basándose en su exclusividad, más que en una superioridad absoluta en puntuación
- Existe un segmento importante de vinos de alta puntuación a precios moderados que pueden promocionarse como excelentes opciones de valor

Esta relación compleja entre precio y puntuación sugiere que tanto productores como comerciantes pueden encontrar éxito enfocándose en la calidad del producto, independientemente del segmento de precio en el que operen. La clave está en comprender que la excelencia en la vinificación puede alcanzarse en cualquier nivel de precio, y que el mercado valora tanto los vinos excepcionales accesibles como los productos de lujo exclusivos.

3. Gráfico de barras → Mejores variedades por puntuación
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.cm as cm
import matplotlib.colors as mcolors
# 3. Top variedades con mejores puntuaciones
# Calcular las 10 variedades con mayor puntuación promedio
top_varieties = df.groupby('variety')['points'].mean().sort_values(ascending=False).head(10)

# Crear el gradiente de colores
norm = mcolors.Normalize(vmin=top_varieties.values.min(), vmax=top_varieties.values.max())
colors = cm.ScalarMappable(norm=norm, cmap="YlOrRd").to_rgba(top_varieties.values)
# Configurar el gráfico de barras
plt.figure(figsize=(12, 6))
sns.barplot(x=top_varieties.values, y=top_varieties.index, palette=colors)
# Agregar títulos y etiquetas con texto en negrita
plt.title('Top 10 Variedades de Vino por Puntuación Promedio', fontsize=16, fontweight='bold')
plt.xlabel('Puntuación Promedio', fontsize=14, fontweight='bold')
plt.ylabel('Variedad', fontsize=14, fontweight='bold')
# Mostrar el gráfico
plt.tight_layout()
plt.show()
El código crea un gráfico de barras con las 10 variedades de vino mejor puntuadas en promedio, facilitando la identificación de las más destacadas en términos de calidad. Esto resulta útil para productores y comerciantes al resaltar las variedades preferidas por su valoración.
Conclusiones del Análisis:
El análisis del top 10 de variedades de vino por puntuación promedio revela patrones significativos en la calidad y preferencia de ciertas variedades. La visualización muestra claramente que Tinta del País, Gelber Traminer y Terrantez se posicionan como las variedades líder, alcanzando las puntuaciones más elevadas. Es particularmente notable la diversidad en este top 10, que incluye tanto variedades tintas como blancas, lo cual sugiere que la excelencia en la vinificación no está limitada a un solo tipo de uva. La distribución de puntuaciones muestra una consistencia en la calidad, con diferencias relativamente pequeñas entre las variedades del ranking, lo que indica un alto estándar general en estas selecciones premium.

Utilidad para Productores Vitivinícolas:
Para los productores de vino, estos resultados ofrecen una valiosa guía para la toma de decisiones estratégicas en sus operaciones. La alta puntuación de variedades como Tinta del País y Gelber Traminer sugiere una oportunidad clara para la especialización y el desarrollo de productos premium. Los productores pueden utilizar esta información para optimizar sus plantaciones, considerando una mayor inversión en las variedades mejor puntuadas. Esta data también justifica la asignación de recursos adicionales para mejorar las técnicas de vinificación específicas para estas variedades destacadas.
Los resultados también proporcionan una base sólida para la planificación a largo plazo. Los productores pueden considerar la diversificación de sus viñedos para incluir más de estas variedades altamente valoradas, o especializar su producción en aquellas que demuestran un rendimiento excepcional. Además, esta información puede respaldar decisiones sobre inversiones en tecnología y equipamiento específico para maximizar la calidad de estas variedades particulares.

Utilidad para Comerciantes de Vino:
Desde la perspectiva comercial, estos datos son extremadamente valiosos para desarrollar estrategias de mercado efectivas. Los comerciantes pueden utilizar estas puntuaciones como una herramienta poderosa para la segmentación de mercado y la fijación de precios. Las variedades mejor puntuadas justifican un posicionamiento premium en el mercado, permitiendo márgenes más altos y la creación de líneas de productos exclusivas.
Los comerciantes pueden desarrollar programas de marketing específicos que destaquen la calidad superior de estas variedades, educando a los consumidores sobre sus características distintivas y valor excepcional. Esta información también es crucial para la gestión de inventario, permitiendo una mejor planificación de compras y la optimización del portfolio de productos. Los comerciantes pueden crear secciones especiales en sus tiendas o cartas de vinos, destacando estas variedades premium y desarrollando relaciones más estrechas con los productores que las cultivan.

Implicaciones para la Colaboración en la Industria:
Estos resultados también sugieren oportunidades significativas para la colaboración entre productores y comerciantes. La consistencia en las altas puntuaciones de estas variedades indica un potencial para desarrollar programas conjuntos de marketing y educación, que beneficiarían a ambas partes de la cadena de valor. Las alianzas estratégicas podrían enfocarse en promover la calidad excepcional de estas variedades, mientras se trabaja en conjunto para mantener y mejorar los estándares de producción y comercialización.

4. Histograma → Distribución de puntuaciones
# 4. Distribución de puntuaciones
plt.figure(figsize=(10, 6))
sns.histplot(data=df, x='points', bins=30, kde=True, color= 'coral')
plt.title('Distribución de Puntuaciones de Vinos')
plt.xlabel('Puntuación')
plt.ylabel('Frecuencia')
plt.tight_layout()
plt.show()
Este código crea un histograma para visualizar la distribución de las puntuaciones de los vinos, mostrando la frecuencia con la que aparecen las diferentes puntuaciones en el conjunto de datos y dando una idea general de la distribución de la calidad del vino. 
El histograma muestra que la mayoría de los vinos tienen puntuaciones entre 85 y 90, sugiriendo una percepción general de calidad favorable.

Análisis de la Distribución de Puntuaciones de Vinos y su Relevancia para la Industria

Descripción del Análisis
El histograma presentado visualiza la distribución de puntuaciones de vinos, mostrando una clara concentración de frecuencias entre los 85 y 90 puntos. La distribución presenta una forma aproximadamente normal con un sesgo hacia puntuaciones más altas, donde se observa una frecuencia máxima de aproximadamente 17,000 vinos en el rango central. Las puntuaciones se extienden desde 80 hasta 100 puntos, con una notable disminución en frecuencia hacia los extremos.

Perspectiva para Productores
Para los productores de vino, esta distribución ofrece información crucial sobre los estándares actuales del mercado. La concentración significativa de vinos entre 85 y 90 puntos establece claramente el nivel de calidad que se ha convertido en el estándar de la industria. Esto sugiere que los productores deben aspirar, como mínimo, a alcanzar los 85 puntos para mantener la competitividad en el mercado.
La distribución también revela un espacio de oportunidad interesante: la frecuencia de vinos que obtienen puntuaciones superiores a 92 puntos disminuye notablemente, con muy pocos alcanzando puntuaciones por encima de 95. Para los productores, esto representa una oportunidad de diferenciación si pueden perfeccionar sus técnicas de producción para alcanzar consistentemente estas puntuaciones superiores.

Implicaciones para Comerciantes
Desde la perspectiva comercial, este histograma proporciona información valiosa para la gestión de inventario y estrategias de precio. La alta concentración de vinos en el rango de 85-90 puntos indica que este es el segmento más común del mercado, sugiriendo que los comerciantes deberían mantener un inventario sólido en este rango para satisfacer la demanda principal.
La distribución muestra también que los vinos con puntuaciones superiores a 92 puntos son significativamente menos comunes, lo que justifica precios premium para estos productos. Los comerciantes pueden utilizar esta escasez relativa para implementar estrategias de precio diferenciadas, especialmente para aquellos vinos que superan los 95 puntos, que según el gráfico son extremadamente raros.

Conclusiones y Recomendaciones
La distribución de puntuaciones revela un mercado maduro con estándares de calidad bien definidos. El hecho de que muy pocos vinos puntúen por debajo de 82 puntos sugiere que existe un umbral mínimo de calidad bien establecido en la industria. Al mismo tiempo, la rareza de puntuaciones extremadamente altas (sobre 95 puntos) indica que hay espacio para la diferenciación en el segmento premium del mercado.

Para los productores, se recomienda:
- Establecer procesos de control de calidad que garanticen consistentemente puntuaciones mínimas de 85 puntos
Invertir en investigación y desarrollo para alcanzar puntuaciones superiores a 92 puntos, donde la competencia es menor.


Para los comerciantes, se sugiere:
- Mantener un inventario base sólido en el rango de 85-90 puntos, que representa el corazón del mercado
- Desarrollar estrategias de precio premium para vinos sobre 92 puntos, respaldadas por su relativa escasez en el mercado
- Utilizar las puntuaciones como herramienta de marketing, especialmente para vinos que se destacan por encima de la media

5. Mapa de calor → Variación de precios por país y región

# 5. Relación Calidad-Precio entre las Principales Variedades de Vino
# Calcular la relación calidad-precio (puntuación / precio)
df['value_ratio'] = df['points'] / df['price']

# Agrupar por las principales variedades de vino 
top_varieties = df.groupby('variety')['value_ratio'].mean().sort_values(ascending=False).head(10)

# Filtrar el dataframe para trabajar solo con las principales variedades
filtered_df = df[df['variety'].isin(top_varieties.index)]

# Configuración de la visualización
plt.figure(figsize=(12, 8))

# Crear un gráfico de dispersión para la relación calidad-precio
sns.scatterplot(x='price', y='points', data=filtered_df, hue='variety', palette='Set2', alpha=0.7, s=100)

# Añadir etiquetas y título
plt.title('Relación Calidad-Precio entre las Principales Variedades de Vino', fontsize=16)
plt.xlabel('Precio del Vino', fontsize=14)
plt.ylabel('Puntuación del Vino', fontsize=14)
# Añadir una leyenda y ajustarla
plt.legend(title='Variedad', bbox_to_anchor=(1.05, 1), loc='upper left')
# Ajustar el layout para una mejor presentación
plt.tight_layout()
# Mostrar el gráfico
plt.show()
El código analiza datos de vinos calculando la relación calidad-precio dividiendo la puntuación entre el precio, identifica las variedades con mejor relación y crea un gráfico de dispersión que muestra cómo se relacionan el precio y la puntuación de estas variedades. En el gráfico, cada punto representa un vino y su color indica la variedad, con el eje X mostrando el precio y el eje Y la puntuación. Esto permite observar si las variedades más caras tienen puntuaciones más altas o si existen opciones con buena calidad-precio en ciertos rangos de precio.
Descripción del Análisis
El gráfico de dispersión muestra la relación entre precio y puntuación para diferentes variedades de vino, con precios que van desde aproximadamente 75 hasta 250 unidades monetarias y puntuaciones entre 80 y 88 puntos. La visualización utiliza un código de colores para distinguir entre variedades como Johannisberg Riesling, Tinto Velasco, Macabeo-Moscatel, y otras variedades importantes del mercado.

Perspectiva para Productores
Analizando el gráfico, los productores pueden observar patrones significativos. El Johannisberg Riesling destaca por mantener puntuaciones consistentemente altas (entre 87-88 puntos) en diferentes niveles de precio, desde los más económicos hasta los más elevados. Esto sugiere que esta variedad en particular logra mantener una calidad superior independientemente del segmento de precio.
Resulta interesante notar que algunas variedades como el Shiraz-Tempranillo muestran una variabilidad significativa en sus puntuaciones (desde 80 hasta 84 puntos), lo que indica que la calidad en esta variedad puede ser más dependiente de factores específicos de producción. Esta información es crucial para los productores al momento de decidir en qué variedades concentrar sus esfuerzos y recursos.


Implicaciones para Comerciantes
Para los comerciantes, el gráfico revela oportunidades interesantes. Por ejemplo, existen vinos Johannisberg Riesling que obtienen puntuaciones superiores a 87 puntos tanto en el rango de precio de 75 como en el de 250 unidades monetarias. Esto sugiere que es posible ofrecer vinos de alta calidad en diferentes segmentos de precio, permitiendo atender diversos perfiles de consumidor.
Es notable que la mayoría de las variedades se concentran en puntuaciones entre 84 y 86 puntos, con precios moderados (alrededor de 75-100 unidades monetarias). Esto representa una oportunidad para los comerciantes de ofrecer vinos de calidad consistente a precios accesibles.

Estrategias de Mercado
Los datos visualizados sugieren estrategias de mercado específicas. Por ejemplo, los comerciantes pueden promover los Johannisberg Riesling de menor precio como opciones de excelente valor, dado que mantienen puntuaciones comparables a sus contrapartes más costosas. Las variedades como Malagouzia-Chardonnay y Macabeo-Moscatel, que muestran puntuaciones sólidas en rangos de precio medios, pueden posicionarse como opciones premium accesibles.

Conclusiones y Recomendaciones
El análisis del gráfico revela que no existe una correlación directa y fuerte entre precio y puntuación para todas las variedades. El Johannisberg Riesling demuestra que es posible mantener alta calidad en diferentes rangos de precio, mientras que otras variedades muestran más variabilidad.
Para los productores, se recomienda estudiar las prácticas de vinificación del Johannisberg Riesling, ya que esta variedad demuestra una consistencia notable en calidad. Para los comerciantes, la estrategia óptima sería mantener un inventario diversificado que incluya:
- Johannisberg Riesling en diferentes rangos de precio, dada su consistente alta calidad
- Una selección de variedades en el rango de 84-86 puntos con precios moderados, que representan el grueso del mercado
- Opciones premium seleccionadas de variedades que demuestran calidad superior en segmentos de precio más altos
