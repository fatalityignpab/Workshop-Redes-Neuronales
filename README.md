# Redes Neuronales

Workshop de Redes Neuronales usando Watson Studio para la UNAM FES Acatlan (*19 de octubre de 2019*) [Presentación]()

![mapa](https://developer.ibm.com/developer/articles/l-neural/images/code_recognizer.gif)

**Diagrama de servicios que maneja Watson Studio**

![mapa](https://developer.ibm.com/developer/articles/introduction-watson-studio/images/02.3-Watson-Studio-Architecture.png)

## Práctica
Construcción una red neuronal para reconocer dígitos escritos a mano utilizando el conjunto de datos MNIST

## Objetivo general
Entrenar un modelo de aprendizaje profundo para reconocer dígitos escritos a mano.

### Objetivos especificos
* Diseñar una [red neuronal convolucional](https://es.wikipedia.org/wiki/Redes_neuronales_convolucionales) (CNN) con una capa convolucional usando el editor de flujo en IBM Watson Studio.
* Entrenar, implementar y probar el modelo usando el generador de experimentos en Watson Studio.

## MNIST

La base de datos MNIST es una gran base de datos de dígitos escritos a mano que se usa comúnmente para capacitar a varios sistemas de procesamiento de imágenes. La base de datos también se usa ampliamente para capacitación y pruebas en el campo del aprendizaje automático.
![Practica](https://3qeqpr26caki16dnhd19sv6by6v-wpengine.netdna-ssl.com/wp-content/uploads/2019/02/Plot-of-a-Subset-of-Images-from-the-MNIST-Dataset.png)



## Prerequisitos

* [Cuenta IBM Cloud](https://cloud.ibm.com/) para crear una cuenta, se detalla en el manual:
    * [Crear una cuenta](https://github.com/fatalityignpab/Workshop-Redes-Neuronales/blob/master/2.%20Academic%20Initiative%20-%20Creating%20an%20IBM%20Cloud%20Account.pdf) (Pueden usar correos personales)
* [Código IBM Cloud](https://my15.digitalexperience.ibm.com/b73a5759-c6a6-4033-ab6b-d9d4f9a6d65b/dxsites/151914d1-03d2-48fe-97d9-d21166848e65/technology/cloud) para generar código de estudiante, se detalla en el manual:
    * [Cómo generar código](https://github.com/fatalityignpab/Workshop-Redes-Neuronales/blob/master/1.%20Academic%20Initiative%20-%20Requesting%20an%20IBM%20Cloud%20Promo%20Code%20(new%20version).pdf) (Solo para Estudiantes o Facultades)
    * [Aplicar código a mi cuenta](https://github.com/fatalityignpab/Workshop-Redes-Neuronales/blob/master/3.%20Academic%20Initiative%20-%20Applying%20an%20IBM%20Cloud%20Promo%20Code.pdf)
* Datos de entrenamiento (Se encuentra en este respositorio, sirve para que la red neuronal pueda ser entrenada con esos datos)
    * mnist-keras-train.pkl
    * mnist-keras-validate.pkl
    * mnist-keras-test.pkl
* Modelo pre-entrenado
    * mnist-keras-test-payload.json

### Herramientas (Servicios)

* [IBM Watson Studio](https://cloud.ibm.com/catalog/services/watson-studio) - Servicio usado para análisis de datos.
* [IBM Watson Machine Learning Service](https://cloud.ibm.com/catalog/services/machine-learning) - Servicio usado para calcular gráficas
* [Cloud Object Storage](https://cloud.ibm.com/catalog/services/cloud-object-storage) - Servicio usado para subir una base de datos no estructurada (noSQL)

Si ya tienen esas herramientas creadas, lo puedes visualizar en su [Lista de recursos > Servicios (Studio y ML) || > Almacenamiento (Object Storage)](https://cloud.ibm.com/resources)

![1](Screenshots/1.png)

## Comenzamos

Para comenzar a realizar la practica, debemos de tener en nuestra cuenta los 3 servicios IBM, para encontrarlos vamos a [Catálogo](https://cloud.ibm.com/catalog):

![2](Screenshots/2.png)

![3](Screenshots/3.png)

Como recomendación, podremos dejar por default al crear los servicios, damos en el boton Crear

![4](Screenshots/4.png)

![5](Screenshots/5.png)

Una vez creado los 3 servicios, podremos dar comienzo a la practica, podremos consultar a nuestra [Lista de recursos](https://cloud.ibm.com/resources).

### Carga de datos

Vamos en el servicio de Cloud Object Storage, donde vamos a crear 2 depositos, uno para almacenar datos de entrenamiento y otro para almacenar resultados de entrenamiento, pueden escribir como default datos-entrenamiento, en la Resiliencia deben de poner Interregional y en la ubicación us-geo (se debe de poner ese ya que el modelo agarrará los datos compatibles en cualquier región), luego dar en "Crear depósito":

![6](Screenshots/6.png)

Una vez creado el deposito, creamos otro, para ello, navegaremos a "Getting started" y en "Creating buckets" y damos en el botón "Crear depósito":

![6.1](Screenshots/6.1.png)

![7](Screenshots/7.png)

Una vez creados, vamos en la sección de "Bucket" y veremos los 2 depositos creados:

![8](Screenshots/8.png)

Accederemos a nuestro deposito de entrenamiento, y ahí agregaremos los 3 datos de entrenamiento:

    * mnist-keras-train.pkl
    * mnist-keras-validate.pkl
    * mnist-keras-test.pkl

Puedes agregar ya sea arrastrar los archivos o subirlos manualmente en el boton "Subir":

![9](Screenshots/9.png)

![10](Screenshots/10.png)

Esperamos a que termine de cargar los dataset (50 MB / 15 minutos aprox.) y ya quedaría este paso.

![11](Screenshots/11.png)

### Creación de una red neuronal

En este tutorial demuestra cómo puede crear un diseño de red neuronal basado en una muestra en la interfaz de usuario del editor de flujo.

Vamos a nuestro servicio Watson Studio y daremos clic en "Get Started"

![12](Screenshots/12.png)

Una vez dentro de Watson Studio, vamos a crear un proyecto, dando clic en "":

![13](Screenshots/13.png)

![14](Screenshots/14.png)

![15](Screenshots/15.png)

![16](Screenshots/16.png)

### Ajuste del servicio Machine Learning

Antes de crear el modelo, debemos de asociar el servicio Machine Learning previamente levantado, para eso, vamos a la opción "Settings":

![16.1](Screenshots/16.1.png)

Vamos en la sección de "Associated services", damos clic en "Add service > Watson"

![16.2](Screenshots/16.2.png)

![16.3](Screenshots/16.3.png)

Seleccionamos el servicio de "Machine Learning" con "Add", nos efocamos a "Existing", seleccionamos el servicio Machine Learning que hemos creado y dar "Select":

![16.4](Screenshots/16.4.png)

![16.5](Screenshots/16.5.png)

### Creación de una red neuronal

Una vez dentro del proyecto, vamos a dar clic en "Add to project" y seleccionamos "Modeler Flow":

![17](Screenshots/17.png)

![18](Screenshots/18.png)

Una vez dentro, vamos a darle "From Example" y seleccionaremos "Single Convolution layer on MNIST" y damos clic en el botón "Create" (Si no les aparece el modelo, intentelo en Modo Incognito):

![19](Screenshots/19.png)

Dentro del modelo, veremos ya prehecho una red neuronal (ver más en [Nodos para Red Neuronal en Watson](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-canvas-nnd-nodes.html))

![20](Screenshots/20.png)

### Actualizar datos del modelo

Una vez dentro del modelo, hay que actualizar el primer nodo para especificar cómo leer los datos de entrenamiento, prueba y validación dando doble clic en ese nodo, donde se desplegará un menú lateral para la configuración:

![21](Screenshots/21.png)

Ahora le damos un nombre a la conexión y clic en el botón "Create a connection", una vez creado, desplegamos la conexión que creamos, seleccionamos el deposito donde añadimos los datos de entrenamiento:

![22](Screenshots/22.png)

![23](Screenshots/23.png)

Damos las propiedades como:
* En el menú desplegable **Train data file**, seleccione "mnist-keras-train.pkl".
* En el menú desplegable **Test data file**, seleccione "mnist-keras-test.pkl".
* En el menú desplegable **Validation data file**, seleccione "mnist-keras-validate.pkl".

![24](Screenshots/24.PNG)

Una vez configurado el nodo, vamos a almacenar el diseño de la red neuronal como una definición de entrenamiento, para eso, vamos a darle en el icono "Publish"

![25](Screenshots/25.PNG)

Le damos nombre al modelo, pueden darle nombre y descripción, en la parte de "Select WML Instance" debemos de asociar al servicio de Machine Learning creado anteriormente en este tutorial, seleccionamos el servicio de ML y se entrenará el modelo:

![26](Screenshots/26.PNG)

**Nota:** Puede descargar el código de construcción de modelos de acuerdo a los sistemas que pueda usar en sus proyectos:

![27](Screenshots/27.PNG)

## Entrena el modelo con un generador de experimentos

Vamos a entrenar nuestro modelo que creamos en un generador de experimentos, debemos de estar en la pantalla principal de nuestro proyecto, en la sección "Asset" damos clic en "New deep learning experiment":

![28](Screenshots/28.PNG)

Se nos abrirá una pantalla donde debemos de dar nombre al experimento y descripción, seleccionamos nuestro servicio Machine Learning y ahora, debemos de configurar nuestros datos de entrenamiento y dato de resultados que ya creamos previamente en este tutorial, damos clic en la parte de "Cloud Object Storage bucket for storing training data files" "Select".

![29](Screenshots/29.PNG)

Nos abríra una ventana donde debemos de especificar a donde está almacenado nuestros datos de entrenamiento, donde especificamos nuestra conexión que hicimos en la parte del modelo, y a que almacenamiento usamos para el entrenamiento donde lo usamos en el modelo, luego damos "Select":

![30](Screenshots/30.PNG)

Al igual haremos lo mismo en el almacenamiento de resultados, donde especificaremos la conexión que usamos en el modelo, pero en el depósito seleccionaremos donde van a arrojar los resultados.

![31](Screenshots/31.PNG)

![32](Screenshots/32.PNG)

Ahora vamos a asignar un modelo de entrenamiento

## Autores

* **Ignacio Pablo Orozco Dávila** - *IBM Advocate* - [LinkedIN](https://www.linkedin.com/in/ignacio-pablo-orozco-d%C3%A1vila-00997315b/)
* **Rafael González Martínez** - *IBM Advocate*
* **Karla Cabañas García** - *IBM Advocate*

### Créditos

[IBM Developer](https://www.ibm.com/developerworks/ssa/index.html)