ulcommerce API v0.1
=======

Flatdoc is a small JavaScript file that fetches Markdown files and renders them
as full pages. Essentially, it's the easiest
way to make open source documentation from *Readme* files.

 * No server-side components
 * No build process needed
 * Deployable via GitHub Pages
 * Can fetch GitHub Readme files
 * Gorgeous default theme (and it's responsive)
 * Just create an HTML file and deploy!

Getting started
---------------

Create a file based on the template, which has a bare DOM, link to the
scripts, and a link to a theme. It will look something like this (not exact).
For GitHub projects, simply place this file in your [GitHub pages] branch and
you're all good to go.

*In short: just download this file and upload it somewhere.*

The main JS and CSS files are also available in [npm] and [bower].

Default theme template>

[Blank template >][blank]

[bower]: http://bower.io/search/?q=flatdoc
[npm]: https://www.npmjs.org/package/flatdoc

<a name="id-introduccion"></a>
# I. Introducción

<a name="id-funcionamientoAPI"></a>
### 1. Funcionamiento de la API
La API de ulcommerce plus es una API REST que utiliza los métodos HTTP para listar, crear, actualizar (POST) o eliminar (DELETE) marcas, productos o categorías de forma individual o masiva. 

La siguiente tabla muestra la estructura general de la API:

| Dependencia | Método | Tipo de Petición | Funcionalidad |
| ------ | ------ | ----- | ----- | ----- |
| Marcas | list-make | post | Listar un registro en particular o todos los existentes. |
| Marcas | create-make | post | Crear uno o varios registros. |
| Marcas | update-make | post | Actualizar uno o varios registros. |
| Marcas | delete-make | delete | Eliminar un registro. |
| Productos | list-product | post | Listar un registro en particular o todos los existentes. |
| Productos | create-product | post | Crear uno o varios registros. |
| Productos | update-product | post | Actualizar uno o varios registros. |
| Productos | delete-product | delete | Eliminar un registro. |
| Categorías  | list-category | post | Listar un registro en particular o todos los existentes. |
| Categorías  | create-category | post | Crear uno o varios registros. |
| Categorías  | update-category | post | Actualizar uno o varios registros. |
| Categorías  | delete-category | delete | Eliminar un registro. |

El esquema básico URI para todas las funciones de la API es: 
http://ulcommerce/api/{version}/{nombre_tienda}/{token}/{metodo}

Especificaciones de los parámetros de la uri:
- **version:** versión de la API que se utilizará para realizar la petición.
- **nombre_tienda:** nombre de la tienda que se utilizará para el manejo de información.
- **token:** identificador de seguridad para la tienda, mayor información consultar el apartado de [Autenticación](#id-autenticacion)
- **metodo:** método que se utilizará para procesar la información de la petición.

Las peticiones y las respuestas se envían con formato JSON.

<a name="id-autenticacion"></a>
### 2. Autenticación
La autenticación al momento de realizar peticiones esta basada en un token propio de cada tienda.

Pasos para obtener el token de la tienda :

- Entrar en la siguiente dirección http://www.ulcommerce.com/developer/
- En la barra de opciones ubicada al costado izquierdo de la página, hacer click en la opción <strong>Tiendas</strong>
( se listar&aacute;n las tiendas configuradas previamente en el administrador de la tienda. )
- En la secci&oacute;n de en medio de la p&aacute;gina aparecer&aacute; por medio de un grid simple el nombre de la tienda, un boton para activar/desactivar la tienda, y un boton para editar la configuraci&oacute;.
- Hacer click en el bot&oacute;n de la derecha para poder acceder al men&uacute; de configuraci&oacute;n.
- En la barra central de opciones seleccionar <strong>configuraci&oacute;n</strong> y posteriormente ir hasta la parte inferior de la pantalla en <strong>Opciones de Desarrollo</strong>

En esta secci&oacute;n se obtiene el token de seguridad que se adiciona a la Api para su correcto funcionamiento, hay dos opciones disponibles. Eliminar tienda y recargar token (esta opcion nos permite generar nuevamente un token aleatorio para el uso privado de la tienda)

<a name="id-codigosEstado"></a>
### 3. Códigos de Estado
Cuando se realiza una petición a la API se muestra un código de estado de HTTP en respuesta a la solicitud. xzEste código de estado proporciona información acerca del estado de la solicitud. 

A continuación se muestran los códigos de estado que podrán ser retornados por la api:

| Código | Descripción |
| ------ | ------ |
| 200 | OK, petición procesada exitosamente. |
| 401 | Unauthorized, se genera al momento de realizar la autenticación y esta falla debido al token, nombre de la tienda o versión de la API. |
| 404 | Not Found, recurso o método no encontrado. |
| 500 | Internal Server.  |

<a name="id-metodos"></a>
# II. Métodos Generales

<a name="id-marcas"></a>
### Gestión de Marcas
En esta sección de la documentación se especificarán los procesos que se deben realizar para listar, crear, actualizar y eliminar marcas de manera correcta por medio de la API.

<a name="id-lisMar"></a>
#####Listar Marcas de Productos 
Ejemplo para listar marcas de productos, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/list-make

> **Nota**: la petición http para este método debe ser de tipo post.

Para listar todas las marcas simplemente se realiza la petición a la url anterior, si se desea una marca específica se debe enviar el siguiente JSON:

```json
    {
        "id": "numero"
    }
```
> **Nota:**"numero" representa el id de la marca que desea listar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-creMar"></a>
#####Creación de Marcas

Ejemplo para creación de marcas, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/create-product

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la creación de las marcas se debe tener en cuenta el [diccionario de datos de creación de marcas](#id-datosCreMar).

Para realizar la creación de marcas se debe tener en cuenta que se pueden crear uno o varios registros a partir de una sola petición a la API. 

El siguiente modelo JSON aplica para la creación de 2 registros, si se desea agregar o insertar solamente un registro se debe modificar según la necesidad:

```json
    {
    "makes":[
                {
                    "name": "Nombre de la Marca",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                },
                {
                    "name": "Nombre de la Marca",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                }    
            ]
    }
```
> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-actMar"></a>
#####Actualización de Marcas

Ejemplo para actualización de marcas, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/update-make

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la actualización de las marcas se debe tener en cuenta el [diccionario de datos de actualización de marcas](#id-datosActMar).

Para la actualización de marcas se debe tener en cuenta que se pueden actualizar uno o varios registros a partir de una sola petición a la API. 

En este método se envían solamente los datos que desea actualizar en el registro (no es necesario enviar todos los datos nuevamente en la petición). 

El siguiente modelo JSON aplica para la actualización de 2 registros, si se desea actualizar un registro o varios se debe modificar según la necesidad:

```json
    {
    "makes":[
                {
                    "id": "25",
                    "name": "Nombre de la Marca Actualizado",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                },
                {
                    "id": "26",
                    "name": "Nombre de la Marca Actualizado",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                }    
            ]
    }
```
> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-eliMar"></a>
#####Eliminación de Marcas

Ejemplo de eliminación de marcas, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/delete-make

> **Nota**: la petición http para este método debe ser de tipo delete.

Para eliminar una marca se debe enviar el id de la marca, ejemplo JSON:

```json
	{
	    "id": "numero"
	}
```
> **Nota:**"numero" representa el id de la marca que desea eliminar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-productos"></a>
###Gestión de Productos
En esta seccion de la documentacion se especificaran los procesos que se deben realizar para listar, crear, actualizar y eliminar productos de manera correcta por medio de la API.
<a name="id-lisPro"></a>
#####Listar Productos

Ejemplo para listar productos, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/list-product

> **Nota**: la petición http para este método debe ser de tipo post.

Para listar todos los productos simplemente se realiza la petición a la url anterior, si se desea un registro específico se debe enviar el siguiente JSON:

```json
    {
        "id": "numero"
    }
```
> **Nota:**"numero" representa el id del producto que desea listar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-crePro"></a>
#####Creación de Productos

Ejemplo para creación de productos, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/create-product

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la creación de productos se debe tener en cuenta el [diccionario de datos de creación de productos ](#id-datosCrePro).

Para realizar la creación de productos se debe tener en cuenta que se pueden crear uno o varios registros a partir de una sola petición a la API.

El siguiente modelo JSON aplica para la creación de 2 registros, si se desea agregar o insertar solamente un registro se debe modificar según la necesidad:

```json
    {
    "products":[
                {
                    "sku": "1",
                    "name": "Nombre del Producto",
                    "description": "Descripción",
                    "shortDescription": "Descripción corta del producto",
                    "price": "100",
                    "salePrice": "120",
                    "freeShipping": "1",
                    "tax": "0",
                    "stock": "10",
                    "make_id": "15"
                },
                {
                    "sku": "1",
                    "name": "Nombre del Producto",
                    "description": "Descripción",
                    "shortDescription": "Descripción corta del producto",
                    "price": "100",
                    "salePrice": "120",
                    "freeShipping": "1",
                    "tax": "0",
                    "stock": "10",
                    "make_id": "22"
                }    
            ]
    }
```
> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-actPro"></a>
#####Actualización de Productos

Ejemplo para actualización de productos, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/update-product

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la creación del producto se debe tener en cuenta el [diccionario de datos de actualización de productos](#id-datosActPro).

Para realizar la actualización de productos se debe tener en cuenta que se pueden actualizar uno o varios registros a partir de una sola petición a la API. 

En este método se envían solamente los datos que desea actualizar en el registro (no es necesario enviar todos los datos del registro nuevamente en la petición).

El siguiente modelo JSON aplica para la actualización de 2 registros, si se desea actualizar un registro o varios se debe modificar según la necesidad:

```json
    {
    "products":[
                {
                    "id": "5",
                    "sku": "1",
                    "name": "Nombre del Producto Actualizado",
                    "description": "Descripción",
                    "shortDescription": "Descripción corta del producto",
                    "price": "100",
                    "salePrice": "120",
                    "freeShipping": "1",
                    "tax": "0",
                    "stock": "10",
                    "make_id": "15"
                },
                {
                    "id": "6",
                    "sku": "1",
                    "name": "Nombre del Producto Actualizado",
                    "description": "Descripción",
                    "shortDescription": "Descripción corta del producto",
                    "price": "100",
                    "salePrice": "120",
                    "freeShipping": "1",
                    "tax": "0",
                    "stock": "10",
                    "make_id": "22"
                }    
            ]
    }
```

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-eliPro"></a>
#####Eliminación de Productos

Ejemplo de eliminación de productos, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/delete-product

> **Nota**: la petición http para este método debe ser de tipo delete.

Para eliminar un producto se debe enviar el id del producto, ejemplo JSON:

```json
	{
	    "id": "numero"
	}
```
> **Nota:**"numero" representa el id del producto que desea eliminar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-categorias"></a>
### Gestion de Categorias
En esta sección de la documentación se especificarán los procesos que se deben realizar para listar, crear, actualizar y eliminar categorías de manera correcta por medio de la API.
<a name="id-lisCat"></a>
#####Listar Categorias

Ejemplo para listar categorias, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/list-category

> **Nota**: la petición http para este método debe ser de tipo post.

Para listar todas las categorías simplemente se realiza la petición a la url anterior, si se desea un registro específico se debe enviar el siguiente JSON:

```json
    {
        "id": "numero"
    }
```
> **Nota:**"numero" representa el id de la categoria que desea listar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-creCat"></a>
#####Creacion de Categorias

Ejemplo para creacion de categorias, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/create-category

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la creación de la categoría se debe tener en cuenta el [diccionario de datos de creación de categorias](#id-datosCreCat).

Para realizar la creacion de categorias se debe tener en cuenta que se pueden crear uno o varios registros a partir de una sola petición a la API. 
El siguiente modelo JSON aplica para la creación de 2 registros, si se desea agregar o insertar solamente un registro se debe modificar según la necesidad:

```json
    {
    "categories":[
                {
                    "parent_id": "1",
                    "name": "Nombre de la categoría",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                },
                {
                    "parent_id": "1",
                    "name": "Nombre de la categoría",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                }    
            ]
    }
```
> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-actCat"></a>
#####Actualización de Categorías

Ejemplo para actualización de categorias, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/update-category

> **Nota**: la petición http para este método debe ser de tipo post.

Al momento de realizar el envío de los datos para la actualización de la categoría se debe tener en cuenta el [diccionario de datos de actualización de categorías](#id-datosActCat).

Para realizar la actualización de categorías se debe tener en cuenta que se pueden crear uno o varios registros a partir de una sola petición a la API. 
En este método se enviarán solamente los datos que desea actualizar en el registro (no es necesario enviar todos los datos del registro nuevamente en la petición).

El siguiente modelo JSON aplica para la creación de 2 registros, si se desea actualizar uno o varios registros se debe modificar según la necesidad:

```json
    {
    "categories":[
                {
                    "id": "1",
                    "parent_id": "1",
                    "name": "Nombre de la categoria",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                },
                {
                    "id": "3",
                    "parent_id": "1",
                    "name": "Nombre de la categoria",
                    "description": "Descripción",
                    "shortDescription": "Descripción Corta",
                    "sequence": "1",
                    "seoTitle": "Titulo SEO",
                    "seoDescription": "Descripción SEO",
                    "meta": "Meta"
                }    
            ]
    }
```

> **Nota**: Para mayor información sobre los posibles errores retornados de la petición consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-eliCat"></a>
#####Eliminación de Categorías

Ejemplo de eliminación de categorías, url de prueba:
http://ulcommerce/api/v0.1/ulc_plus/01542e2b2bd0bba14/delete-category

> **Nota**: la petición http para este método debe ser de tipo delete.

Para eliminar una categoría se debe enviar el id de la categoría, ejemplo JSON:

```json
	{
	    "id": "numero"
	}
```
> **Nota:** "numero" representa el id de la categoria que desea eliminar.

> **Nota**: Para mayor información sobre los posibles errores retornados de la peticion consulte el [diccionario de errores](#id-diccionarioErrores).

<a name="id-opciones"></a>
### Gestión de Opciones
En esta sección de la documentación se especificarán cada una de las opciones disponibles para configurar todas las variables que afectan la tienda.

Las opciones de configuración se dividen en dos secciones principales.
- Configuración de Ordenes.
- Configuración General.

## 1. Configuración de Ordenes
Por medio de esta configuración se tiene acceso a:

### - Estado de Ordenes.
Aqui informacion de estado de ordenes.

<a name="id-anexos"></a>
# III. Anexos
<a name="id-diccionarioDatos"></a>
### 1. Diccionarios de datos
El Diccionario de Datos es la tabla que relaciona y describe las variables que deben ser registradas en la API cada por el usuario para cada uno de los procesos.

El Diccionario de Datos contiene las características de registro que se deben cumplir, como el tamaño de la variable, el tipo de dato y si es requerido o no su registro. También contiene la validación realizada por la API para determinar si el dato registrado es correcto.

<a name="id-datosCreMar"></a>
##### Diccionario de datos de Creación de Marcas

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| name | Nombre de la Marca | S | Varchar | 180 |
| description | Descripción | N | Text | N/A |
| shortDescription | Descripción Corta | S | Varchar | 140 |
| sequence | Orden de los Elementos | N | Integer | 10 |
| seoTitle | Título para Seo | N | Text | N/A |
| seoDescription | Descripción para Seo | N | Text | N/A |
| meta | Metadatos para Seo | N | Text | N/A |

<a name="id-datosActMar"></a>
##### Diccionario de datos de Actualización de Marcas

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| id | Id del Producto | S | Integer | N/A |
| name | Nombre de la Marca | S | Varchar | 180 |
| description | Descripción | N | Text | N/A |
| shortDescription | Descripción Corta | S | Varchar | 140 |
| sequence | Orden de los elementos | N | Integer | 10 |
| seoTitle | Título para Seo | N | Text | N/A |
| seoDescription | Descripción para Seo | N | Text | N/A |
| meta | Metadatos para Seo | N | Text | N/A |

<a name="id-datosCrePro"></a>
##### Diccionario de datos de Creación de Productos

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| sku | Número de referencia de producto | N | Text | N/A |
| name | Nombre del Producto | S | Varchar | 180 |
| description | Descripción del producto | N | Text | N/A |
| shortDescription | Descripción corta del producto | N | Varchar | 140 |
| price | Precio | N | Float | N/A |
| salePrice | Precio de venta | N | Float | N/A |
| freeShipping | Indicador envío gratis | N | Boolean | N/A |
| tax | Indicador de impuesto | N | Boolean | N/A |
| stock | Número de productos disponibles en el inventario | N | Decimal | 10 |
| make_id | Id de la marca | S | Integer | N/A |

<a name="id-datosActPro"></a>
##### Diccionario de datos de Actualización de Productos

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| id | Id del Producto | S | Integer | N/A |
| sku | Número de referencia de producto | N | Text | N/A |
| name | Nombre del Producto | N | Varchar | 180 |
| description | Descripción del producto | N | Text | N/A |
| shortDescription | Descripción corta del producto | N | Varchar | 140 |
| price | Precio | N | Float |  N/A |
| salePrice | Precio de venta | N | Float | N/A |
| freeShipping | Indicador envío gratis | N | Boolean | N/A |
| tax | Indicador de impuesto | N | Boolean | N/A |
| stock | Número de productos disponibles en el inventario | N | Decimal | 10 |
| make_id | Id de la marca | N | Integer | N/A |

<a name="id-datosCreCat"></a>
##### Diccionario de datos de Creación de Categorías

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| parent_id | -- | N | Integer | N/A |
| name | Nombre de la Categoria | S | Varchar | 180 |
| description | Descripción | N | Text | N/A |
| shortDescription | Descripción Corta | S | Varchar | 140 |
| sequence | Secuencia | N | Integer | 10 |
| seoTitle | Título del Seo | N | Text | N/A |
| seoDescription | Descripción Seo | N | Text | N/A |
| meta | -- | N | Text | N/A |

<a name="id-datosActCat"></a>
##### Diccionario de datos de Actualización de Categorías

| Nombre Campo | Descripción | Requerido | Tipo Dato | Tamaño |
|----|----|----|----|----|
| id | Id de la categoria | S | Integer | N/A |
| parent_id | -- | N | Integer | N/A |
| name | Nombre de la Categoría | N | Varchar | 180 |
| description | Descripción | N | Text | N/A |
| shortDescription | Descripción Corta | N | Varchar | 140 |
| sequence | Secuencia | N | Integer | 10 |
| seoTitle | Titulo del Seo | N | Text | N/A |
| seoDescription | Descripción Seo | N | Text | N/A |
| meta | -- | N | Text | N/A |

<a name="id-diccionarioErrores"></a>
### 2. Diccionarios de Errores
El Diccionario de Errores es la tabla que relaciona y describe los errores que genera la API cuando alguna variable no ha sido registrada de forma correcta en alguno de los metodos.

El Diccionario de Errores contiene la descripción del error y su posible solución.

| Texto Error | Solución |
|----|----|
| Authentication Failed. | Este error se presenta debido a que alguna de las variables enviadas en la URI (version, nombre de la  tienda, token) no es correcta. Se debe realizar una verificación de los datos enviados en la URI. |
| Check the information submitted. | Este error se presenta debido a que la estructura enviada en el JSON no es correcta. Se debe realizar la verificación de la estructura dada como ejemplo en el correspondiente instructivo de la función. |
| [item] not found. | Este error se presenta en el momento en que se envía un dato y se realiza su previa verificación pero no se encuentra el registro en la base de datos. Se debe realizar la verificación del item enviado. |
| No records found. | Este error se presenta en el momento en que se realiza la petición de un listado de items y no se encuentra ninguno de estos en la base de datos. La solución para este error es realizar la petición siempre y cuando se encuentre al menos un registro en la base de datos. |
| The [item_externo] is incorrect. | Este error se presenta en el momento en que se desea asociar la creación o actualización de un item con un dato perteneciente a otra tabla. Se debe realizar laverificaciónn de este item y comprobar que realmente exista en la tabla de referencia. |

<a name="id-glosario"></a>
### 3. Glosario
- **Diccionario de Datos:** El Diccionario de Datos es la tabla que relaciona y describe las variables que deben ser registradas en la API cada por el usuario para cada uno de los procesos. El Diccionario de Datos contiene las características de registro que se deben cumplir, como el tamaño de la variable, el tipo de dato y si es requerido o no su registro. También contiene la validación realizada por la API para determinar si el dato registrado es correcto.
- **Diccionario de Errores:** El Diccionario de Errores es la tabla que relaciona y describe los errores que genera la API cuando alguna variable no ha sido registrada de forma correcta en alguno de los métodos. El Diccionario de Errores contiene la descripción del error, la variable al que pertenece y su solución.
- **Marcas:**
- **Productos:**
- **Categorías:**
- **Json:** acrónimo de JavaScript Object Notation, es un formato ligero para el intercambio de datos.
- **Token:** Identificador único de una aplicación que solicita el acceso a su servicio. 
