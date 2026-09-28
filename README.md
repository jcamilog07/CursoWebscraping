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
