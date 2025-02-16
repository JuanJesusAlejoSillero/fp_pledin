---
title: "Práctica: Instalación/migración de aplicaciones web PHP"
---

## Descripción

### Escenario

Crea un escenario con vagrant o kvm con las siguiente características:

* Dos máquinas virtuales que se llamen `servidor_web_tunombre` y `servidor_bd_tunombre`.
* La máquina `servidor_web_tunombre` estará conectada una red pública.
* Las dos máquinas están conectadas entre si por una red muy aislada.

## Tarea 1: Instalación de un CMS PHP en mi servidor local

* Selecciona un CMS escrito en PHP para desplegarlo en nuestra infraestructura. No se puede elegir ni WordPress (que lo hemos visto en un vídeo, ni NextCloud que lo instalaremos a continuación).
* Configura en la máquina `servidor_web_tunombre` un servidor web apache2 que ejecute PHP, con un VirtualHost, para que el CMS sea accesible desde la dirección: `www.nombrealumno.org`.
* Configura en la máquina `servidor_bd_tunombre` una base de datos.  Crea un usuario con privilegios sobre la base de datos donde se van a guardar los datos del CMS. Configura la base de datos para que permita conexión desde la otra máquina.
* Descarga el CMS seleccionado y realiza la instalación.
* Realiza una configuración mínima de la aplicación (Cambia la plantilla, crea algún contenido, ...)
* Instala un módulo para añadir alguna funcionalidad al CMS.

## Tarea 2: Instalación de un CMS NextCloud

* Instala el CMS PHP NextCloud en otro host virtual con el que accedemos con el nombre `cloud.nombrealumno.org`.

## Tarea 3: Migración a tu VPS

* Configura en tu VPS un servidor LEMP.
* Configura un registro DNS en tu servidor DNS de tipo **CNAME** para que el nombre `www` apunte al nombre de vuestro servidor.
* Realiza la migración de tus aplicaciones web a tu VPS. La primera aplicación debe ser accesible desde la URL `www.tudominio.algo/portal`, y el NextCloud con la URL `www.tudominio.algo/cloud`. ¿Cuántos virtual hosts tendrás que configurar en el servidor web de tu VPS?
* Al acceder a `www.tudominio.algo` se debe hacer una redirección a `www.tudominio.algo/portal`.
*  Instala en un ordenador el cliente de NextCloud y realiza la configuración adecuada para acceder a "tu nube".


{% capture notice-text %}
## Entrega

1. De la Tarea 1, entrega la configuración del virtualhost.
2. De la Tarea 1, la configuración de resolución estática.
3. De la Tarea 1, una captura de pantalla donde se vea el acceso a la aplicación.
4. De la Tarea 1, indica que plugin has instalado.
5. De la Tarea 2, una captura de pantalla donde se vea el acceso a la aplicación.
6. De la Tarea 3, documenta de la forma más precisa posible cada uno de los pasos que has dado para migrar una de las dos aplicaciones.
7. De la Tarea 3, las URL de acceso a las aplicaciones.
8. Capturas de pantalla donde se demuestre que esta funcionando el cliente de NextCloud.
{% endcapture %}<div class="notice--info">{{ notice-text | markdownify }}</div>
