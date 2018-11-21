---
title: Introducción a Visual Search
author: scseely
ms.author: scseely, mazoroto
ms.date: 07/16/2018
ms.topic: article
ms.service: industry
description: En este artículo se explican las fases de migración de la infraestructura de comercio electrónico del entorno local a Azure.
ms.openlocfilehash: 0c80e3068a1b23bf12d2468489fdd0b67c660dfa
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654212"
---
# <a name="visual-search-overview"></a>Introducción a Visual Search

## <a name="executive-summary"></a>Resumen ejecutivo

Como todos ya sabemos, la inteligencia artificial ofrece el potencial de transformar el comercio minorista. Es razonable creer que los minoristas desarrollarán una arquitectura de la experiencia del cliente respaldada por la inteligencia artificial. Algunas de las expectativas son que una plataforma mejorada con inteligencia artificial proporcionará un aumento notable de los ingresos debido a la hiperpersonalización. El comercio digital continúa reforzando las expectativas, las preferencias y el comportamiento del cliente. Demandas como interacción en tiempo real, recomendaciones pertinentes e hiperpersonalización están impulsando la velocidad y la comodidad al clic de un botón. La habilitación de la inteligencia en las aplicaciones mediante voz natural, visión, etc. permite mejoras en el sector minorista que aumentarán el valor y afectaran al modo en que los clientes compran.

Este documento se centra en el concepto de inteligencia artificial de **búsqueda visual** y ofrece algunas consideraciones importantes sobre su implementación. Se proporciona un flujo de trabajo de ejemplo y se asignan sus fases a las tecnologías de Azure pertinentes. El concepto se basa en un cliente que puede aprovechar una imagen tomada con su dispositivo móvil o ubicada en Internet para realizar una búsqueda de artículos pertinentes o parecidos según la intención de la experiencia. Por lo tanto, la búsqueda visual mejora la velocidad desde la entrada de texto a una imagen con varios puntos de metadatos a fin de que aparezcan rápidamente los artículos aplicables disponibles.

## <a name="visual-search-engines"></a>Motores de búsqueda visual

Los motores de búsqueda visual recuperan información mediante el uso de imágenes como entrada y, con frecuencia, aunque no exclusivamente, también como salida.

Los motores son cada vez más comunes en el sector minorista y por muy buenas razones:

- Según un informe publicado por [Emarketer](https://www.emarketer.com/Report/Visual-Commerce-2017-How-Image-Recognition-Augmentation-Changing-Retail/2002059) en 2017, casi un 75 % de los usuarios de Internet buscan imágenes o vídeos de un producto antes de realizar una compra.
- De acuerdo con el informe de [Slyce](https://slyce.it/wp-content/uploads/2015/11/Visual_Search_Technology_and_Market.pdf) (una compañía de búsqueda visual) de 2015, el 74 % de los consumidores también considera que las búsquedas de texto no son eficaces.

Por lo tanto, según la investigación realizada por [Markets &amp; Markets](https://www.marketsandmarkets.com/PressReleases/image-recognition.asp), el mercado de reconocimiento de imágenes representará más de 25 000 millones de dólares USD para el año 2019.

La tecnología ya ha arraigado en las principales marcas de comercio electrónico, quienes también han contribuido significativamente a su desarrollo. Los usuarios pioneros más importantes son probablemente:

- eBay con sus herramientas Image Search y "Find It on eBay" de la aplicación (por ahora solo una experiencia móvil).
- Pinterest con su herramienta de detección visual, Lens.
- Microsoft con Bing Visual Search.

## <a name="adopt-and-adapt"></a>Adopción y adaptación

Afortunadamente, no se necesitan grandes cantidades de potencia de computación para beneficiarse de la búsqueda visual. Cualquier negocio con un catálogo de imágenes puede sacar partido de la experiencia de inteligencia artificial de Microsoft integrada en sus servicios de Azure.

[Bing Visual Search](https://azure.microsoft.com/en-us/services/cognitive-services/bing-visual-search/?WT.mc_id=vsearchgio-article-gmarchet) API proporciona una manera de extraer información de contexto de las imágenes e identificar, por ejemplo, mobiliario para el hogar, moda, varias clases de productos, etc.

También devuelve imágenes de su propio catálogo que son visualmente parecidas, productos con orígenes de compra relativos o búsquedas relacionadas. Si bien es interesante, su uso será limitado si la empresa no es uno de esos orígenes.

Bing también proporciona:

- Etiquetas que le permiten explorar objetos o conceptos que se encuentran en la imagen.
- Rectángulos de selección para regiones de interés de la imagen (por ejemplo, artículos de ropa o muebles).

Puede tomar esa información para reducir considerablemente el espacio (y el tiempo) de búsqueda en el catálogo de productos de la empresa y limitarlo a los objetos que son como los de la región y categoría de interés.

## <a name="implement-your-own"></a>Implementación por su cuenta

Hay algunos componentes clave que debe tener en cuenta al implementar la búsqueda visual:

- Ingesta y filtrado de imágenes
- Técnicas de almacenamiento y recuperación
- Caracterización, codificación o "configuración de hash"
- Medidas o distancias de similitud y clasificación

 ![](./assets/visual-search-use-case-overview/visual-search-pipeline.png)

*Figura 1: Ejemplo de canalización de Visual Search*

### <a name="sourcing-the-pictures"></a>Origen de las imágenes

Si no dispone de un catálogo de imágenes, es posible que deba entrenar los algoritmos en conjuntos de datos disponibles públicamente, como fashion [MNIST](https://www.kaggle.com/zalando-research/fashionmnist), deep [fashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html) y similar. Estos algoritmos contienen varias categorías de productos y se usan normalmente para comparar los algoritmos de clasificación y búsqueda de imágenes.

 ![](./assets/visual-search-use-case-overview/deep-fashion-dataset.png)

*Figura 2: Ejemplo del conjunto de datos Deep Fashion*

### <a name="filtering-the-images"></a>Filtrado de las imágenes

La mayoría de los conjuntos de datos de referencia, como los antes mencionados, se han procesado previamente.

Si va a compilar el suyo propio, como mínimo, le interesará que todas las imágenes tengan el mismo tamaño, determinado principalmente por la entrada para la que se entrena el modelo.

En muchos casos, es mejor también normalizar la luminosidad de las imágenes. Según el nivel de detalle de la búsqueda, color puede ser también información redundante, por lo que reducirlo a blanco y negro le ayudará con los tiempos de procesamiento.

Por último, pero no menos importante, el conjunto de datos de imagen debe estar equilibrado entre las diferentes clases que representa.

### <a name="image-database"></a>Base de datos de imágenes

La capa de datos es un componente especialmente delicado de su arquitectura. Contendrá:

- Imágenes
- Los metadatos sobre las imágenes (tamaño, etiquetas, SKU del producto o descripción)
- Los datos generados por el modelo de aprendizaje automático (por ejemplo, un vector numérico de 4096 elementos por imagen)

A medida que recupera imágenes de distintos orígenes o usa modelos de aprendizaje automático para obtener un rendimiento óptimo, la estructura de los datos cambia. Por lo tanto, es importante elegir una tecnología o una combinación que pueda tratar con datos semiestructurados y sin esquema fijo.

También puede necesitar un número mínimo de puntos de datos útiles (por ejemplo, un identificador o clave de imagen, una SKU de producto, una descripción o un campo de etiqueta).

[Azure Cosmos DB](https://azure.microsoft.com/en-us/services/cosmos-db/?WT.mc_id=vsearchgio-article-gmarchet) ofrece la flexibilidad necesaria, junto con diversos mecanismos de acceso para las aplicaciones basadas en este servicio (lo que ayudará a la búsqueda en el catálogo). Sin embargo, hay que tener cuidado para alcanzar la mejor relación precio/rendimiento. Cosmos DB permite el almacenamiento de los datos adjuntos de los documentos, pero hay un límite total por cuenta y puede ser una propuesta costosa. Una práctica común es almacenar los archivos de imagen reales en blobs e insertar un vínculo a ellos en la base de datos. En el caso de Cosmos DB, esto supone crear un documento que contenga las propiedades del catálogo asociadas a esa imagen (SKU, etiqueta, etc.) y un archivo adjunto que contenga la dirección URL del archivo de imagen (por ejemplo, en Azure Blob Storage, OneDrive, etc.).

 ![](./assets/visual-search-use-case-overview/cosmosdb-data-model.png)

*Figura 3: Modelo jerárquico de recursos de Cosmos DB*

Si tiene previsto aprovechar las ventajas de la distribución global de Cosmos DB, tenga en cuenta que se replicarán los documentos y datos adjuntos, pero no los archivos vinculados. Para ello, puede considerar una red de distribución de contenido.

Otras tecnologías aplicables son una combinación de Azure SQL Database (si es aceptable el esquema fijo) y blobs, o incluso tablas de Azure y blobs para almacenamiento y recuperación de forma rápida y económica.

### <a name="feature-extraction-amp-encoding"></a>Extracción de características y codificación

El proceso de codificación extrae características destacadas de las imágenes de la base de datos y asigna a cada una de ellas un "vector de características" disperso (un vector con muchos ceros) que puede tener miles de componentes. Este vector es una representación numérica de las características (por ejemplo, bordes o formas) que caracterizan la imagen, de forma parecida a un código.

Las técnicas de extracción de características usan normalmente _mecanismos de aprendizaje por transferencia_. Esto tiene lugar cuando selecciona una red neuronal previamente entrenada, ejecuta cada imagen a través de ella y almacena el vector de características de nuevo en la base de datos de imágenes. En este modo, el aprendizaje se "transfiere" desde la persona que haya entrenado la red. Microsoft ha desarrollado y publicado varias redes previamente entrenadas que se han usado ampliamente en las tareas de reconocimiento de imágenes, como [ResNet50](https://www.kaggle.com/keras/resnet50).

Dependiendo de la red neuronal, el vector de características será más o menos largo y disperso, de ahí que los requisitos de memoria y almacenamiento varíen.

Además, puede encontrarse con que diferentes redes son aplicables a diferentes categorías, de ahí que una implementación de la búsqueda visual pueda generar en realidad vectores de características de diverso tamaño.

Las redes neuronales previamente entrenadas son relativamente fáciles de usar pero podrían no ser tan eficaces como un modelo personalizado entrenado en el catálogo de imágenes. Estas redes están diseñadas normalmente para la clasificación de conjuntos de datos de referencia y no para realizar búsquedas en una colección de imágenes específica.

Como es posible que quiera modificarlas y volverlas a entrenar de modo que generen una predicción de categorías y un vector denso (es decir, más pequeño, no disperso), lo que sería muy útil para restringir el espacio de búsqueda, reduzca los requisitos de almacenamiento y memoria. Se pueden usar vectores binarios y con frecuencia se conocen como "[hash semántico](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf)": un término derivado de las técnicas de codificación y recuperación de documentos. La representación binaria simplifica la realización de cálculos adicionales.

 ![](./assets/visual-search-use-case-overview/resnet-modifications.png)

*Figura 4: Modificaciones de ResNet para Visual Search (F. Yang et al, 2017)*

Si elige modelos previamente entrenados o prefiere desarrollar los suyos propios, también deberá decidir dónde ejecutar la caracterización o el entrenamiento del modelo propiamente dicho.

Azure ofrece varias opciones: máquinas virtuales, Azure Batch, [Batch AI](https://azure.microsoft.com/en-us/services/batch-ai/?WT.mc_id=vsearchgio-article-gmarchet) o clústeres de Databricks. Sin embargo, en todos los casos, la mejor relación precio/rendimiento viene dada por el uso de GPU.

Microsoft ha anunciado recientemente la disponibilidad de matrices de puertas programables, o FPGA, para el cálculo rápido por una mínima parte del costo de GPU (proyecto [Brainwave](https://www.microsoft.com/en-us/research/blog/microsoft-unveils-project-brainwave/?WT.mc_id=vsearchgio-article-gmarchet)). Sin embargo, en el momento de escribir este artículo, esta oferta está limitada a determinadas arquitecturas de red, por lo que deberá evaluar su rendimiento detenidamente.

### <a name="similarity-measure-or-distance"></a>Medida o distancia de similitud

Cuando las imágenes se representan en el espacio del vector de característica, encontrar similitudes se convierte en una cuestión de definir una medida de distancia entre los puntos de dicho espacio. Una vez que se ha definido una distancia, puede calcular los clústeres de imágenes similares o definir matrices de similitud. En función de la métrica de distancia seleccionada, los resultados pueden variar. La medida de distancia euclidiana más común sobre vectores de números reales, por ejemplo, es fácil de entender: captura la magnitud de la distancia. Sin embargo, es bastante ineficaz en términos de cálculo.

La distancia [coseno](https://en.wikipedia.org/wiki/Cosine_similarity) se usa a menudo para capturar la orientación del vector, en lugar de su magnitud.

Alternativas como la distancia de [Hamming](https://en.wikipedia.org/wiki/Hamming_distance) a través de representaciones binarias renuncian a cierta precisión a favor de eficiencia y velocidad.

La combinación de medida de distancia y tamaño de vector determinará la cantidad de cálculo y memoria que consumirá la búsqueda.

### <a name="search-amp-ranking"></a>Búsqueda y clasificación

Una vez que se ha definido la similitud, es necesario idear un método eficaz para recuperar los N elementos más cercanos al que se ha pasado como entrada y, luego, devolver una lista de identificadores. Esto también se conoce como "clasificación de imágenes". En un conjunto de datos grande, el tiempo para calcular cada distancia es prohibitivo, por lo que se usan algoritmos vecinos aproximados. Para ellos existen varias bibliotecas de código abierto, por lo que no tendrá que escribir el código desde cero.

Por último, los requisitos de memoria y cálculo determinarán la elección de la tecnología de implementación para el modelo entrenado, así como la alta disponibilidad. Normalmente, el espacio de búsqueda se particionará y varias instancias del algoritmo de clasificación se ejecutarán en paralelo. Una opción que permite escalabilidad y disponibilidad son los clústeres de [Azure Kubernetes](https://azure.microsoft.com/en-us/services/container-service/kubernetes/?WT.mc_id=vsearchgio-article-gmarchet). En ese caso, es conveniente implementar el modelo de clasificación en varios contenedores (de modo que cada uno controle una partición del espacio de búsqueda) y varios nodos (para lograr una alta disponibilidad).

## <a name="next-steps"></a>Pasos siguientes

La implementación de la búsqueda visual no tiene que ser compleja. Puede usar Bing o crear la suya propia con los servicios de Azure, mientras se beneficia de la investigación sobre inteligencia artificial de Microsoft y las herramientas desarrolladas para esta tecnología.

### <a name="trial"></a>Versión de prueba

- Pruebe la [consola de prueba de Visual Search API](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac).

### <a name="develop"></a>Desarrollo

- Para empezar a crear un servicio personalizado, consulte [Introducción a Bing Visual Search API](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview/?WT.mc_id=vsearchgio-article-gmarchet).
- Para crear su primera solicitud, consulte las guías de inicio rápido: [C#](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/csharp) | [Java](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/java) | [node.js](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/nodejs) | [Python](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/python)
- Familiarícese con la [referencia de Visual Search API](https://aka.ms/bingvisualsearchreferencedoc).

### <a name="background"></a>Fondo

- [Deep Learning Image Segmentation](https://www.microsoft.com/developerblog/2018/04/18/deep-learning-image-segmentation-for-ecommerce-catalogue-visual-search/?WT.mc_id=vsearchgio-article-gmarchet) (Segmentación de imágenes de aprendizaje profundo): documento de Microsoft que describe el proceso de separar las imágenes de los fondos.
- [Visual Search at Ebay](https://arxiv.org/abs/1706.03154) (Búsqueda visual en Ebay): investigación de la Universidad de Cornell.
- [Visual Discovery at Pinterest](https://arxiv.org/abs/1702.04680) (Detección visual en Pinterest): investigación de la Universidad de Cornell.
- [Semantic Hashing](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf) (Hash semántico): investigación de la Universidad de Toronto.

_En este artículo fue creado por Giovanni Marchetti y Mariya Zorotovich._