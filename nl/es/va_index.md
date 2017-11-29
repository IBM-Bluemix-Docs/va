---

copyright:
  years: 2017
lastupdated: "2017-10-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip} 
{:download: .download}

# Gestión de la seguridad de imágenes con Vulnerability Advisor 
{: #va_index}

Vulnerability Advisor comprueba el estado de seguridad de las imágenes de contenedor antes del despliegue.
{:shortdesc}


## Acerca de Vulnerability Advisor 
{: #about}

Vulnerability Advisor proporciona funciones de gestión de la seguridad para {{site.data.keyword.containerlong}}. Vulnerability Advisor genera un informe de estado de la seguridad, sugiere arreglos y prácticas recomendadas y ofrece funciones de gestión para restringir la ejecución de las imágenes que no son seguras. Arreglar los problemas de seguridad y configuración de los que informa Vulnerability Advisor puede ayudarle a proteger la infraestructura de {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Vulnerability Advisor incluye las siguientes características:

-   Explora imágenes para detectar vulnerabilidades
-   Ofrece un informe de evaluación basado en estándares de seguridad, como ISO 27002, así como prácticas de seguridad específicas de {{site.data.keyword.containerlong_notm}}
-   Detecta los programas maliciosos basados en archivos
-   Ofrece recomendaciones para proteger los archivos de configuración de un subconjunto de tipos de aplicación
-   Proporciona instrucciones sobre cómo solucionar un problema de vulnerabilidad o de configuración notificado en sus informes

<dl>
  <dt><strong>Paquetes vulnerables</strong></dt>
  <dd>Vulnerability Advisor comprueba si hay paquetes vulnerables en imágenes basadas en sistemas operativos soportados y proporciona un enlace con los avisos de seguridad relevantes acerca de la vulnerabilidad. Vulnerability Advisor actualiza diariamente su lista interna con estos avisos de seguridad. Las imágenes base soportadas se describen en la tabla siguiente:</dd>
</dl>

  |Imagen base de Docker|Origen de los avisos de seguridad|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://git.alpinelinux.org/) y [CIRCL CVE Search ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.circl.lu/)|
  |CentOS| [Archivos de anuncios de CentOS ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-announce/) y [Archivos de anuncios de CentOS CR ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Anuncios de seguridad de Debian ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Avisos de seguridad de Ubuntu ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ubuntu.com/usn/)|
  {: caption="Tabla 1. Imágenes base de Docker que Vulnerability Advisor comprueba en busca de paquetes vulnerables" caption-side="top"}










## Revisión de la seguridad de imágenes de Docker para imágenes que se han almacenado en un espacio de nombres en {{site.data.keyword.registrylong_notm}} mediante la interfaz de línea de mandatos 
{: #va_registry_cli}

Puede revisar la seguridad de imágenes de Docker que se han almacenado en un espacio de nombres para buscar información sobre vulnerabilidades potenciales.
{:shortdesc}

Cuando añade una imagen a {{site.data.keyword.registrylong_notm}}, Vulnerability Advisor la explora automáticamente para detectar problemas de seguridad y potenciales vulnerabilidades. Los contenedores que se despliegan desde las imágenes vulnerables pueden ser atacados y estar en peligro. Las imágenes solo se exploran si se basan en un sistema operativo que recibe soporte de Vulnerability Advisor.

Vulnerability Advisor comprueba las siguientes vulnerabilidades. Si encuentra problemas de seguridad, se proporcionan instrucciones para poder arreglar el problema de vulnerabilidad notificado.

Para comprobar el estado de vulnerabilidad de imágenes en su cuenta {{site.data.keyword.Bluemix_notm}}, lleve a cabo los pasos siguientes:

1.  Enumere todas las imágenes en su cuenta {{site.data.keyword.Bluemix_notm}}. El siguiente mandato devuelve una lista de todas las imágenes independientemente del espacio de nombres del lugar en el que se almacenan.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Compruebe el estado en la columna VULNERABILITY STATUS. Se visualiza uno de los siguientes estados:
    -   Bien. Este estado significa que no se han encontrado problemas de seguridad.
    -   Vulnerable. Este estado significa que se ha encontrado un problema potencial de seguridad o de vulnerabilidad.
    -   Desconocido. Este estado se muestra mientras la imagen se está explorando hasta que se determina el estado de vulnerabilidad final.
    -   SO no soportado. Este estado se visualiza si Vulnerability Advisor no da soporte a la exploración de la imagen.
4.  Para obtener más información sobre el estado, revise el informe de Vulnerability Advisor.

    ```
    bx cr va registry.<región>/<mi_espaciodenombres>/<mi_imagen>:<etiqueta>
    ```
    {: pre}

    En su salida de la CLI, puede encontrar la lista de paquetes vulnerables, una descripción de la vulnerabilidad que se ha encontrado, y un enlace para obtener las instrucciones de cómo corregirla.


## Resolución de problemas en imágenes 
{: #va_report}

Arreglos de ejemplo para problemas comunes de los que informa Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor proporciona acciones correctivas junto con los problemas de seguridad o de configuración de los que informa. Algunos de los problemas notificados pueden arreglarse actualizando Dockerfile. Para ayudar a resolver problemas comunes de forma más rápida, utilice los siguientes ejemplos de código para implementar la solución en el Dockerfile.

### Antigüedad máxima de la contraseña, días mínimos para la contraseña y longitud mínima de la contraseña
{: #va_password}

Problema: Recibe uno de los siguientes errores, o todos ellos:

```
La antigüedad máxima de la contraseña debe establecerse en 90 días.
```
{: screen}

```
La longitud mínima de la contraseña debe ser de 8.
```
{: screen}

```
El mínimo de días que deben transcurrir entre cambios de contraseña iniciados por el usuario debe ser de 1.
```
{: screen}

Arreglo: Establezca la conformidad de las contraseñas añadiendo a su Dockerfile el código siguiente.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnerabilidad de SSH 
{: #ssh}

Problema: Se devuelve el siguiente error:

```
No debe estar instalado el servidor de SSH.
```
{: screen}

Arreglo: En lugar de utilizar SSH, utilice `docker attach` o `docker exec` para acceder a su contenedor. Asegúrese de que el Dockerfile no contiene ningún paso para la instalación de un servidor de SSH.

