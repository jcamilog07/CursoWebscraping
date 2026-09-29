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

## Flujo de un proyecto

```text
Enviar solicitud → Recibir HTML → Analizar la página
→ Extraer datos → Limpiar y organizar → Guardar resultados

# Web Scraping de libros con Python

## Descripción del proyecto

Este proyecto es un ejercicio introductorio de **web scraping** con Python. El programa visita la página de prueba [Books to Scrape](https://books.toscrape.com/), recibe su contenido HTML y extrae información de los libros que aparecen en la página principal.

De cada libro se recopilan estos datos:

- Nombre
- Precio
- URL de la imagen

Al final, los resultados se guardan en un archivo CSV llamado `productos.csv`.

> **Nota:** Books to Scrape es un sitio de práctica para aprender web scraping.

## Herramientas utilizadas

- **Python:** lenguaje con el que se escribe el programa.
- **Requests:** biblioteca que envía solicitudes HTTP y recibe respuestas.
- **Beautiful Soup:** biblioteca que analiza el HTML y permite localizar elementos.
- **CSV:** formato de archivo para organizar datos en filas y columnas.
- **Jupyter Notebook:** entorno interactivo donde se ejecuta el código paso a paso.

## Requisitos previos

Antes de comenzar, necesitas tener:

- Python instalado.
- Jupyter Notebook o VS Code con soporte para notebooks.
- Conexión a internet para acceder al sitio web.

## Paso 1: instalar las bibliotecas

Abre una terminal y ejecuta:

```bash
python -m pip install requests beautifulsoup4
```

Si trabajas dentro de un notebook de Jupyter, puedes instalar las bibliotecas en una celda:

```python
%pip install requests beautifulsoup4
```

`requests` permite hacer solicitudes a sitios web. `beautifulsoup4` permite analizar el HTML recibido y buscar los elementos que contienen los datos.

## Paso 2: importar las bibliotecas

Instalación de la librería "requests" Esta librería permite realizar peticiones HTTP e interactuar de forma sencilla con sitios y servicios web.

```python
!pip3 install requests
```

¿Qué hace cada importación?

- `requests`: solicita la página web.
- `BeautifulSoup`: analiza el contenido HTML.
- `csv`: escribe los datos en un archivo CSV.
- `os`: crea la carpeta donde se guardarán los resultados.
- `urljoin`: combina la dirección de la página con una ruta relativa para formar una URL completa.

## Paso 3: guardar la dirección del sitio

Define la página que quieres consultar:

```python
url = "https://books.toscrape.com/"
```

La variable `url` contiene la dirección principal del catálogo.

## Paso 4: enviar una solicitud al sitio web

Usa el método `GET` de Requests para pedir el contenido de la página:

```python
response = requests.get(url)
```

La variable `response` contiene lo que devolvió el servidor. Puedes comprobar el código de estado:

```python
print(response.status_code)
```

Algunos códigos frecuentes:

- `200`: la solicitud fue exitosa.
- `404`: no se encontró la página o el recurso.
- `429`: se enviaron demasiadas solicitudes en poco tiempo.
- `500`: ocurrió un problema en el servidor.

Comprueba que la página respondió correctamente antes de continuar:

```python
if response.status_code == 200:
    print("Conexión exitosa")
else:
    print("Error en la conexión:", response.status_code)
```

## Paso 5: revisar el contenido HTML

El contenido de la página se puede consultar con `response.text`:

```python
print(response.text[:500])
```

Este ejemplo muestra los primeros 500 caracteres para no imprimir toda la página en el notebook. El HTML contiene la estructura de la página: títulos, enlaces, imágenes y otros elementos.

## Paso 6: analizar el HTML con Beautiful Soup

Convierte el HTML recibido en un objeto que Beautiful Soup pueda recorrer:

```python
soup = BeautifulSoup(response.text, "html.parser")
```

El argumento `"html.parser"` indica que el contenido debe interpretarse como HTML.

Ahora puedes inspeccionar la página. Por ejemplo, busca el título que aparece en la pestaña del navegador:

```python
titulo_pagina = soup.find("title")

if titulo_pagina:
    print(titulo_pagina.get_text(strip=True))
```

`find()` busca el primer elemento que coincida con la etiqueta indicada. `get_text(strip=True)` obtiene su texto y quita espacios al principio y al final.

También puedes buscar el encabezado principal del sitio:

```python
encabezado = soup.find("div", class_="col-sm-8 h1")

if encabezado:
    print(encabezado.get_text(strip=True))
```

## Paso 7: localizar los libros

En Books to Scrape, cada libro está dentro de un elemento `<article>` que tiene la clase `product_pod`.

Usa un selector CSS para encontrar todos esos elementos:

```python
products = soup.select("article.product_pod")

print(f"Libros encontrados: {len(products)}")
```

El selector `article.product_pod` significa “busca los elementos `<article>` cuya clase sea `product_pod`”.

La variable `products` contiene una lista con los libros encontrados en la página consultada.

## Paso 8: extraer nombre, precio e imagen

Crea una lista vacía. En ella guardarás los datos de cada libro:

```python
product_list = []
```

Después, recorre cada elemento de `products` y extrae la información:

```python
for product in products:
    # Obtener el nombre completo desde el atributo title
    nombre = product.find("h3").find("a")["title"]

    # Obtener el precio y eliminar espacios sobrantes
    precio = product.find(
        "p",
        class_="price_color"
    ).get_text(strip=True)

    # Obtener la ruta de la imagen
    imagen_relativa = product.find(
        "div",
        class_="image_container"
    ).find("img")["src"]

    # Convertir la ruta relativa en una URL completa
    imagen_url = urljoin(url, imagen_relativa)

    # Guardar los tres datos en la lista
    product_list.append({
        "nombre": nombre,
        "precio": precio,
        "imagen_url": imagen_url
    })
```

### ¿Cómo se obtiene cada dato?

- `product.find("h3").find("a")["title"]` localiza el enlace del título y obtiene el nombre completo del libro.
- `product.find("p", class_="price_color")` localiza el párrafo que contiene el precio.
- `.get_text(strip=True)` devuelve el texto sin espacios sobrantes.
- `product.find("div", class_="image_container").find("img")["src"]` encuentra la imagen y obtiene su dirección desde el atributo `src`.
- `urljoin(url, imagen_relativa)` combina la URL principal con la ruta de la imagen.
- `product_list.append(...)` añade la información del libro a la lista.

Puedes comprobar cuántos libros se guardaron en la lista:

```python
print(f"Productos extraídos: {len(product_list)}")
```

También puedes ver algunos resultados:

```python
for libro in product_list[:5]:
    print(libro)
```

`product_list[:5]` muestra solo los primeros cinco libros.

## Paso 9: crear la carpeta de resultados

Antes de escribir el CSV, crea la carpeta `resultados`:

```python
os.makedirs("resultados", exist_ok=True)
```

`exist_ok=True` evita un error si la carpeta ya existe.

## Paso 10: guardar los datos en un CSV

Define la ruta del archivo y escribe los datos:

```python
path_csv = "resultados/productos.csv"

with open(path_csv, "w", newline="", encoding="utf-8") as archivo:
    columnas = ["nombre", "precio", "imagen_url"]

    writer = csv.DictWriter(
        archivo,
        fieldnames=columnas
    )

    writer.writeheader()
    writer.writerows(product_list)

print(
    f"Extracción completa: {len(product_list)} productos "
    f"guardados en {path_csv}"
)
```

¿Qué hace esta parte?

- `open(..., "w")` crea el archivo o reemplaza su contenido si ya existe.
- `newline=""` ayuda a evitar líneas vacías adicionales en algunos programas.
- `encoding="utf-8"` permite guardar correctamente caracteres especiales.
- `csv.DictWriter(...)` escribe diccionarios en formato CSV.
- `fieldnames=columnas` define los nombres y el orden de las columnas.
- `writeheader()` escribe la primera fila con los encabezados.
- `writerows(product_list)` escribe una fila por cada libro.

El archivo generado queda en:

```text
resultados/productos.csv
```

## Código completo

Este es el código completo del proceso, desde la solicitud hasta la creación del CSV:

```python
import requests
from bs4 import BeautifulSoup
import csv
import os
from urllib.parse import urljoin

# Dirección de la página que se va a consultar
url = "https://books.toscrape.com/"

# Enviar una solicitud GET
response = requests.get(url)

# Comprobar que la solicitud fue exitosa
if response.status_code == 200:
    print("Conexión exitosa")

    # Analizar el HTML recibido
    soup = BeautifulSoup(response.text, "html.parser")

    # Buscar todos los libros de la página
    products = soup.select("article.product_pod")

    # Lista para almacenar los datos extraídos
    product_list = []

    # Recorrer los libros y extraer sus datos
    for product in products:
        nombre = product.find("h3").find("a")["title"]

        precio = product.find(
            "p",
            class_="price_color"
        ).get_text(strip=True)

        imagen_relativa = product.find(
            "div",
            class_="image_container"
        ).find("img")["src"]

        imagen_url = urljoin(url, imagen_relativa)

        product_list.append({
            "nombre": nombre,
            "precio": precio,
            "imagen_url": imagen_url
        })

    # Crear la carpeta de resultados si todavía no existe
    os.makedirs("resultados", exist_ok=True)

    # Guardar la información en un archivo CSV
    path_csv = "resultados/productos.csv"

    with open(path_csv, "w", newline="", encoding="utf-8") as archivo:
        columnas = ["nombre", "precio", "imagen_url"]

        writer = csv.DictWriter(
            archivo,
            fieldnames=columnas
        )

        writer.writeheader()
        writer.writerows(product_list)

    print(
        f"Extracción completa: {len(product_list)} productos "
        f"guardados en {path_csv}"
    )

else:
    print("Error en la conexión:", response.status_code)
```

## Estructura sugerida del proyecto

Organiza los archivos del proyecto de esta manera:

```text
web-scraping-libros/
├── Curso Webscraping.ipynb
├── resultados/
│   └── productos.csv
├── requirements.txt
└── README.md
```

El archivo `requirements.txt` puede contener:

```text
requests
beautifulsoup4
```

Así, otra persona puede instalar las bibliotecas necesarias con:

```bash
python -m pip install -r requirements.txt
```

## Cómo ejecutar el proyecto

1. Descarga o clona el repositorio.
2. Abre la carpeta del proyecto en VS Code o Jupyter.
3. Instala las bibliotecas desde la terminal o con `requirements.txt`.
4. Abre `Curso Webscraping.ipynb`.
5. Ejecuta las celdas en orden, desde la primera hasta la última.
6. Cuando termine el proceso, busca el archivo `resultados/productos.csv`.

## Alcance del ejemplo

Este ejercicio extrae los libros que aparecen en la página principal del sitio. No recorre las demás páginas del catálogo.

## Buenas prácticas

- Revisa las condiciones de uso del sitio antes de recopilar información.
- Haz solicitudes con moderación y evita enviar muchas peticiones seguidas.
- Comprueba el código de estado antes de analizar la respuesta.
- Los selectores dependen de la estructura HTML; si el sitio cambia, puede ser necesario actualizarlos.
- No subas contraseñas, claves privadas ni información personal a GitHub.
- Usa este ejemplo con fines educativos y respeta las reglas del sitio.

## Resultado

Al finalizar, el proyecto genera un archivo CSV con una fila por cada libro encontrado y tres columnas:

| Columna | Contenido |
|---|---|
| `nombre` | Nombre del libro |
| `precio` | Precio mostrado en la página |
| `imagen_url` | Dirección completa de la imagen |
