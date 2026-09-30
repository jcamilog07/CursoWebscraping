# Introducción al Web Scraping

El **web scraping** es una técnica para recopilar información de páginas web de forma automática. Un programa visita una página, descarga su contenido y extrae los datos que necesitamos, como nombres, precios, descripciones o enlaces a imágenes.

Se utiliza para analizar información pública, hacer comparaciones o crear conjuntos de datos. Antes de usarlo, revisa las condiciones del sitio y evita recopilar datos personales o enviar demasiadas solicitudes.

## ¿Cómo funciona?

1. El programa envía una **solicitud HTTP** a una página web.
2. El **servidor** recibe la solicitud y devuelve una respuesta.
3. La respuesta puede incluir el contenido de la página, normalmente en **HTML**.
4. El programa busca en ese contenido los datos que necesita.
5. Los resultados se organizan y se guardan, por ejemplo, en un archivo **CSV**.

## Conceptos básicos

- **Cliente:** programa que solicita información. En este proyecto, es el código de scraping.
- **Servidor:** sistema que aloja la página web y responde a las solicitudes.
- **HTTP/HTTPS:** protocolos que permiten la comunicación entre el cliente y el servidor. HTTPS añade una conexión segura.
- **Solicitud (request):** petición que el cliente envía al servidor.
- **Respuesta (response):** resultado que el servidor devuelve, junto con un código de estado.
- **HTML:** estructura de una página web. Sus elementos organizan textos, enlaces, imágenes y otros contenidos.
- **Etiqueta (tag):** elemento de HTML, como `<h1>`, `<p>` o `<a>`.
- **Atributo:** información adicional dentro de una etiqueta. Por ejemplo, `href` indica la dirección de un enlace.
- **Selector:** patrón que permite localizar elementos de una página, como una clase CSS o una etiqueta HTML.
- **Parsear:** leer y organizar el HTML para encontrar los datos deseados.
- **CSV:** formato de archivo tabular que permite guardar datos en filas y columnas.

## Códigos HTTP frecuentes

- **200:** la solicitud se procesó correctamente.
- **404:** no se encontró la página o el recurso.
- **429:** se enviaron demasiadas solicitudes en poco tiempo.
- **500:** ocurrió un error en el servidor.

## Herramientas comunes en Python

- **`requests`:** envía solicitudes HTTP y recibe respuestas.
- **Beautiful Soup (`bs4`):** ayuda a analizar HTML y localizar elementos.
- **`csv` o `pandas`:** permiten organizar y guardar los datos extraídos.




## Importar las bibliotecas

Instalación de la librería "requests" Esta librería permite realizar peticiones HTTP e interactuar de forma sencilla con sitios y servicios web.

```python
!pip3 install requests
```

Instalación de libreria "beautifulsoup4" esta libreria nos permite interactuar con cada uno de los elementos del sitio web estatico.

```python
!pip3 install beautifulsoup4 
```

¿Cómo funciona una página web?
Cuando entras a una página, tu computador le pide información a otro computador llamado servidor. El servidor responde enviando la página o los datos.
Es como pedir algo en una tienda: tú haces el pedido y la tienda responde. El código de estado indica cómo salió la solicitud.
- Code 200 — Todo salió bien: el servidor encontró lo que pediste y te lo envió.
- Code 404 — No se encontró: el servidor no encontró la página o el recurso que buscabas.
- Code 400 — La solicitud está mal escrita: el servidor no entiende lo que le pediste.
- Code 422 — Hay un dato incorrecto: la solicitud se entiende, pero uno de los datos enviados no sirve.
- Code 429 — Hiciste demasiadas solicitudes: espera un poco antes de volver a intentarlo.
- Code 500 — Hubo un problema en el servidor: el problema está en la página o en el computador que la envía.
- Code 496 — Problema con el certificado de seguridad: algunos servidores usan este código cuando hay un problema con la conexión segura.
La comunicación entre tu computador y el servidor suele usar HTTP, un conjunto de reglas para pedir y enviar información por internet.

```python
import requests
from bs4 import BeautifulSoup
import csv
```

# Hacer peticiones HTTP
### CONCEPTOS

### HTTP:

- 🌎 Hypertext Transfer Protocol.
- Es el protocolo de comunicación que permite las transferencias de información a través de archivos en la World Wide Web.

### GET:

- 🔍 Recupera datos del servidor.
- Se usa para leer o consultar información. No modifica nada.

### POST:

- ✉️ Envía datos al servidor.
- Se usa para crear nuevos recursos (por ejemplo, enviar un formulario).

### PUT:

- 🛠️ Actualiza un recurso existente.
- Reemplaza por completo el recurso con la nueva información enviada.

### DELETE:

- ❌ Elimina un recurso del servidor.
- Se usa para borrar datos específicos.

```python
url = "http://books.toscrape.com/"
```
```python
# Realizar la petición GET
response = requests.get(url)
```
```python
# Verificar el código de estado
print(response)
```
```python
<Response [200]>
```
```python
# Trae la pagina en formato HTML
print(response.text)
```
```python
<!DOCTYPE html>
<!--[if lt IE 7]>      <html lang="en-us" class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html lang="en-us" class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html lang="en-us" class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html lang="en-us" class="no-js"> <!--<![endif]-->
    <head>
        <title>
    All products | Books to Scrape - Sandbox
</title>

        <meta http-equiv="content-type" content="text/html; charset=UTF-8" />
        <meta name="created" content="24th Jun 2016 09:29" />
        <meta name="description" content="" />
        <meta name="viewport" content="width=device-width" />
        <meta name="robots" content="NOARCHIVE,NOCACHE" />

        <!-- Le HTML5 shim, for IE6-8 support of HTML elements -->
        <!--[if lt IE 9]>
        <script src="//html5shim.googlecode.com/svn/trunk/html5.js"></script>
        <![endif]-->

            <link rel="shortcut icon" href="static/oscar/favicon.ico" />
...
        
    </body>
</html>
```
```python
if response.status_code == 200:
    print("Conexión exitosa. Código de estado:", response.status_code)
    # Imprimir los primeros 500 caracteres del HTML
    print(response.text[0:500])
else:
    print("Error en la conexión. Código de estado:", response.status_code)

```
```python

Conexión exitosa. Código de estado: 200
<!DOCTYPE html>
<!--[if lt IE 7]>      <html lang="en-us" class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html lang="en-us" class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html lang="en-us" class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html lang="en-us" class="no-js"> <!--<![endif]-->
    <head>
        <title>
    All products | Books to Scrape - Sandbox
</title>

```

# Explorar la estructura HTML con BeautifulSoup
### Conceptos
🧱 ¿Qué es el DOM?
DOM (Document Object Model) es una representación estructurada del contenido HTML de una página web, en forma de un árbol jerárquico de nodos.

🧠 ¿Para qué sirve?
El DOM permite que lenguajes de programación como JavaScript o Python (con BeautifulSoup, por ejemplo) puedan:

- Acceder a elementos del HTML(< div >,< h1 >,< p >, etc.)
- Leer o modificar contenido.
- Navegar entre elementos (padres, hijos, hermanos).
- Automatizar la extracción o manipulación de datos.


```python

#🌳 ¿Cómo luce el DOM?
#Por ejemplo, este HTML:

#<html>
#  <body>
#    <h1>// Curso de Scraping</h1> ------- TITULO, si hubiera un h2 fuera un subtitulo 
#    <p>Aprende a extraer datos web</p> -----PARRAFO
#  </body>
#</html>
#Se representa en el DOM como este árbol:

#html
#└── body
#    ├── h1 → "Curso de Scraping"
#    └── p → "Aprende a extraer datos web"
#Cada etiqueta es un nodo, y puede contener texto u otros nodos.
```

✨ ¿Por qué es importante en scraping?
Porque para extraer información de una página web, necesitas saber dónde está ubicada en el DOM (por ejemplo, seleccionar todos los productos dentro de un < div class="producto">).

```python

url = "http://books.toscrape.com/"
```
```python
# Realizar la petición GET
response = requests.get(url)
```
```python
soup = BeautifulSoup(response.text, "html.parser")
```
```python
print(soup)
```
```python
<!DOCTYPE html>

<!--[if lt IE 7]>      <html lang="en-us" class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html lang="en-us" class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html lang="en-us" class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en-us"> <!--<![endif]-->
<head>
<title>
    All products | Books to Scrape - Sandbox
</title>
<meta content="text/html; charset=utf-8" http-equiv="content-type"/>
<meta content="24th Jun 2016 09:29" name="created"/>
<meta content="" name="description"/>
<meta content="width=device-width" name="viewport"/>
<meta content="NOARCHIVE,NOCACHE" name="robots"/>
<!-- Le HTML5 shim, for IE6-8 support of HTML elements -->
<!--[if lt IE 9]>
        <script src="//html5shim.googlecode.com/svn/trunk/html5.js"></script>
        <![endif]-->
<link href="static/oscar/favicon.ico" rel="shortcut icon"/>
<link href="static/oscar/css/styles.css" rel="stylesheet" type="text/css"/>
<link href="static/oscar/js/bootstrap-datetimepicker/bootstrap-datetimepicker.css" rel="stylesheet"/>
<link href="static/oscar/css/datetimepicker.css" rel="stylesheet" type="text/css"/>
</head>
<body class="default" id="default">
...
<!-- Version: N/A -->
</body>
</html>
```
```python
# Extraer el head
head = soup.find("title")
print(head.get_text(strip=True))
```
```python

All products | Books to Scrape - Sandbox
```

```python

# Extraer el título principal
titulo = soup.find("div", class_="col-sm-8 h1")
print(titulo.get_text(strip=True))
```
```python
Books to ScrapeWe love being scraped!
```

## Extracción de productos, imagenes, nombre y precios

```python
url = "http://books.toscrape.com/"
```
```python
# Realizar la petición GET
response = requests.get(url)
```
```python
soup = BeautifulSoup(response.text, "html.parser")
```
```python

# Buscar todos los productos
products = soup.select("article.product_pod")
```
```python

print(products)
```
```python
[<article class="product_pod">
<div class="image_container">
<a href="catalogue/a-light-in-the-attic_1000/index.html"><img alt="A Light in the Attic" class="thumbnail" src="media/cache/2c/da/2cdad67c44b002e7ead0cc35693c0e8b.jpg"/></a>
</div>
<p class="star-rating Three">
<i class="icon-star"></i>
<i class="icon-star"></i>
<i class="icon-star"></i>
<i class="icon-star"></i>
<i class="icon-star"></i>
</p>
<h3><a href="catalogue/a-light-in-the-attic_1000/index.html" title="A Light in the Attic">A Light in the ...</a></h3>
<div class="product_price">
<p class="price_color">Â£51.77</p>
<p class="instock availability">
<i class="icon-ok"></i>
    
        In stock
    
</p>
<form>
<button class="btn btn-primary btn-block" data-loading-text="Adding..." type="submit">Add to basket</button>
</form>
</div>
</article>, <article class="product_pod">
...
<button class="btn btn-primary btn-block" data-loading-text="Adding..." type="submit">Add to basket</button>
</form>
</div>
</article>]
```
```python
# Lista para almacenar la información
product_list = []

for product in products:
    # Nombre del libro
    nombre = product.find("h3").find("a")["title"]
    print(nombre)
```
```python
A Light in the Attic
Tipping the Velvet
Soumission
Sharp Objects
Sapiens: A Brief History of Humankind
The Requiem Red
The Dirty Little Secrets of Getting Your Dream Job
The Coming Woman: A Novel Based on the Life of the Infamous Feminist, Victoria Woodhull
The Boys in the Boat: Nine Americans and Their Epic Quest for Gold at the 1936 Berlin Olympics
The Black Maria
Starving Hearts (Triangular Trade Trilogy, #1)
Shakespeare's Sonnets
Set Me Free
Scott Pilgrim's Precious Little Life (Scott Pilgrim #1)
Rip it Up and Start Again
Our Band Could Be Your Life: Scenes from the American Indie Underground, 1981-1991
Olio
Mesaerion: The Best Science Fiction Stories 1800-1849
Libertarianism for Beginners
It's Only the Himalayas
```
```python
# Lista para almacenar la información
product_list = []

for product in products:
    # Nombre del libro
    nombre = product.find("h3").find("a")["title"]
    print(nombre)
    
    # Precio
    precio = product.find("p", class_="price_color").get_text()
    print(precio)
    
    # Imagen
    imagen = product.find("div", class_="image_container").find("img")["src"]
    imagen_url = "http://books.toscrape.com/" + imagen
    print(imagen_url)
```
```python
A Light in the Attic
Â£51.77
http://books.toscrape.com/media/cache/2c/da/2cdad67c44b002e7ead0cc35693c0e8b.jpg
Tipping the Velvet
Â£53.74
http://books.toscrape.com/media/cache/26/0c/260c6ae16bce31c8f8c95daddd9f4a1c.jpg
Soumission
Â£50.10
http://books.toscrape.com/media/cache/3e/ef/3eef99c9d9adef34639f510662022830.jpg
Sharp Objects
Â£47.82
http://books.toscrape.com/media/cache/32/51/3251cf3a3412f53f339e42cac2134093.jpg
Sapiens: A Brief History of Humankind
Â£54.23
http://books.toscrape.com/media/cache/be/a5/bea5697f2534a2f86a3ef27b5a8c12a6.jpg
The Requiem Red
Â£22.65
http://books.toscrape.com/media/cache/68/33/68339b4c9bc034267e1da611ab3b34f8.jpg
The Dirty Little Secrets of Getting Your Dream Job
Â£33.34
http://books.toscrape.com/media/cache/92/27/92274a95b7c251fea59a2b8a78275ab4.jpg
The Coming Woman: A Novel Based on the Life of the Infamous Feminist, Victoria Woodhull
Â£17.93
http://books.toscrape.com/media/cache/3d/54/3d54940e57e662c4dd1f3ff00c78cc64.jpg
The Boys in the Boat: Nine Americans and Their Epic Quest for Gold at the 1936 Berlin Olympics
...
http://books.toscrape.com/media/cache/0b/bc/0bbcd0a6f4bcd81ccb1049a52736406e.jpg
It's Only the Himalayas
Â£45.17
http://books.toscrape.com/media/cache/27/a5/27a53d0bb95bdd88288eaf66c9230d7e.jpg
```
```python
import os
import csv

path_csv = "resultados/productos.csv"
os.makedirs("resultados", exist_ok=True)

with open(path_csv, "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(
        f,
        fieldnames=["nombre", "precio", "imagen_url"]
    )
    writer.writeheader()
    writer.writerows(product_list)

print(f"Extracción completa: {len(product_list)} productos guardados en {path_csv}")
```
