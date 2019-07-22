---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-03"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, containers, security issues, configuration issues,

subcollection: va

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# Gestión de la seguridad de imágenes con Vulnerability Advisor
{: #va_index}

Vulnerability Advisor comprueba el estado de seguridad de las imágenes de contenedor proporcionadas por {{site.data.keyword.IBM}}, terceros, o que se han añadido al espacio de nombres del registro de la organización.
{:shortdesc}

Cuando añade una imagen a un espacio de nombres, Vulnerability Advisor la explora automáticamente para detectar problemas de seguridad y potenciales vulnerabilidades. Si encuentra problemas de seguridad, se proporcionan instrucciones para poder arreglar el problema de vulnerabilidad notificado.

Vulnerability Advisor proporciona funciones de gestión de la seguridad para [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) y genera un informe sobre el estado de la seguridad que incluye correcciones sugeridas y procedimientos recomendados.

Todos los problemas que encuentra Vulnerability Advisor dan lugar a un veredicto que indica que no es recomendable desplegar esta imagen. Si elige desplegar la imagen, todos los contenedores que se despliegan de la imagen incluirán problemas conocidos que podrían utilizarse en un ataque o bien comprometer el contenedor. El veredicto se ajusta en función de las exenciones que ha especificado. Este veredicto lo puede utilizar Container Image Security Enforcement para evitar el despliegue de imágenes no seguras en {{site.data.keyword.containerlong_notm}}.

Corregir los problemas de seguridad y configuración de los que informa Vulnerability Advisor le ayudará a proteger la infraestructura de {{site.data.keyword.cloud_notm}}.

## Acerca de Vulnerability Advisor
{: #about}

Vulnerability Advisor proporciona varias funciones que le ayudarán a proteger sus imágenes.
{:shortdesc}

Están disponibles las siguientes funciones:

- Explora imágenes para encontrar problemas
- Proporciona un informe de evaluación basado en prácticas de seguridad específicas de {{site.data.keyword.containerlong_notm}}
- Ofrece recomendaciones para proteger los archivos de configuración de un subconjunto de tipos de aplicación
- Proporciona instrucciones sobre cómo solucionar un [paquete vulnerable](#packages) o un [problema de configuración](#app_configurations) notificado en sus informes
- Proporciona un veredicto a [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Aplica exenciones a los informes de una cuenta, espacio de nombres o repositorio, o a nivel de etiqueta para saber qué problemas no se aplican a su caso

En el panel de control Registro, la columna **Estado de política** muestra el estado de sus repositorios. El informe enlazado identifica prácticas recomendadas de seguridad en la nube para sus imágenes.

El panel de control de Vulnerability Advisor muestra una visión general y una evaluación de la seguridad para una imagen. Si desea obtener más información sobre el panel de control de Vulnerability Advisor, consulte [Revisión de un informe de vulnerabilidad](#va_reviewing).

**Protección de los datos**

Para explorar imágenes y contenedores en su cuenta por motivos de seguridad, Vulnerability Advisor recopila, almacena y procesa la información siguiente:

- Campos de formato libre, incluidos ID, descripciones y nombres de imágenes (etiqueta de registro, de espacio de nombres, de nombre de repositorio y de imagen)
- Metadatos sobre las modalidades de archivos y la creación de indicaciones de fecha y hora de los archivos de configuración
- El contenido del sistema y de los archivos de configuración de aplicación en imágenes y contenedores
- Paquetes y bibliotecas instalados (incluidas sus versiones)

No coloque información personal en ningún campo ni ubicación que procese Vulnerability Advisor, tal como se identifica en la lista anterior.

Los resultados de la exploración, agregados en un nivel de centro de datos, se procesan para crear métricas anonimizadas para operar y mejorar el servicio.

Los resultados de la exploración se suprimen a los 30 días una vez generados.

## Tipos de vulnerabilidades
{: #types}

### Paquetes vulnerables
{: #packages}

Vulnerability Advisor comprueba si hay paquetes vulnerables en imágenes que utilizan sistemas operativos soportados y proporciona un enlace con los avisos de seguridad relevantes acerca de la vulnerabilidad.
{:shortdesc}

Se muestran los paquetes que contienen problemas de vulnerabilidad conocidos en los resultados de la exploración. Las posibles vulnerabilidades se actualizan a diario utilizando los avisos de seguridad publicados para los tipos de imagen de Docker que se muestran en la tabla siguiente. Por lo general, para que un paquete vulnerable pase la exploración, se necesita una versión posterior del paquete que incluya una corrección para la vulnerabilidad en cuestión. El mismo paquete puede tener varias vulnerabilidades; en tal caso, una única actualización del paquete puede corregir varias vulnerabilidades.

  |Imagen base de Docker|Origen de los avisos de seguridad|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://git.alpinelinux.org/) y [CIRCL CVE Search ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.circl.lu/).|
  |CentOS| [Archivos de anuncios de CentOS ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-announce/) y [Archivos de anuncios de CentOS CR ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-cr-announce/). Para obtener más información sobre vulnerabilidades, consulte [Vulnerabilidades en paquetes en CentOS](#va_centos).|
  |Debian|[Anuncios de seguridad de Debian ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.debian.org/debian-security-announce/).|
  |Red Hat Enterprise Linux (RHEL)|[API de datos de seguridad de Red Hat ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://access.redhat.com/labsinfo/securitydataapi).|
  |Ubuntu|[Avisos de seguridad de Ubuntu ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/).|
  {: caption="Tabla 1. Imágenes base de Docker soportadas que Vulnerability Advisor comprueba en busca de paquetes vulnerables" caption-side="top"}

### Problemas de configuración
{: #app_configurations}

Los problemas de configuración son problemas de seguridad potenciales relacionados con cómo se configura una app. Muchos de los problemas notificados pueden arreglarse actualizando Dockerfile.
{:shortdesc}

Las imágenes solo se exploran si utilizan un sistema operativo que recibe soporte de Vulnerability Advisor. Vulnerability Advisor comprueba los valores de configuración para los siguientes tipos de apps:

- MySQL
- NGINX
- Apache

## Revisión de un informe de vulnerabilidad
{: #va_reviewing}

Antes de desplegar una imagen, puede revisar su informe de Vulnerability Advisor para ver los detalles sobre los paquetes vulnerables y valores de configuración no seguros de la app o del contenedor. También puede comprobar si la imagen cumple con las políticas organizativas.
{:shortdesc}

Si no soluciona ningún problema descubierto, los problemas pueden afectar a la seguridad de los contenedores creados utilizando dicha imagen. Si Container Image Security Enforcement no se despliega, puede seguir utilizando una imagen que tenga problemas de seguridad y de configuración en un contenedor. Si Container Image Security Enforcement se despliega y se activa para la imagen, todos los problemas descubiertos deben estar exentos mediante la política para que los contenedores sean desplegables desde esta imagen.

Para configurar el ámbito de obligatoriedad de los problemas de Vulnerability Advisor en Container Image Security Enforcement, consulte [Personalización de políticas](/docs/services/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

Si la imagen no cumple los requisitos establecidos por la política de su organización, debe configurar la imagen para que cumpla dichos requisitos para poder desplegarla. Para obtener más información sobre cómo ver y cambiar la política de organización, consulte [Establecimiento de políticas de exención organizativas](#va_managing_policy).
{:tip}

### Revisión de un informe de vulnerabilidad utilizando la interfaz gráfica de usuario
{: #va_reviewing_gui}

Puede revisar la seguridad de imágenes de Docker que se han almacenado en sus espacios de nombres de {{site.data.keyword.registrylong_notm}} mediante la GUI.
{:shortdesc}

1. Inicie una sesión en {{site.data.keyword.cloud_notm}}.
2. Pulse el icono de **Menú de navegación** y pulse **Kubernetes**.
3. Pulse **Registro** y pulse el mosaico **Imágenes**. Se muestra una lista de sus imágenes y el estado de seguridad en la tabla **Imágenes**.
4. Para ver el informe de la imagen con la etiqueta `latest`, pulse la fila correspondiente a esa imagen. Se abre el separador **Detalles de imagen**, que muestra los datos correspondientes a dicha imagen. Si no existe la etiqueta `latest` en el repositorio, se utilizará la imagen más reciente.
5. Si es estado de seguridad muestra algún problema, para encontrar información pulse el separador **Problemas por tipo**. Se abren las tablas **Vulnerabilidades** y **Problemas de configuración**.

   - Tabla **Vulnerabilidades**: Muestra el ID de vulnerabilidad de cada problema, el estado de la política para dicho problema, los paquetes afectados y cómo solucionar el problema. Para ver más información para este problema, amplíe la fila. Se muestra un resumen del problema, que contiene un enlace con el aviso de seguridad del proveedor correspondiente a dicho problema. Muestra una lista de los paquetes que contienen problemas de vulnerabilidad conocidos.
  
     La lista se actualiza a diario utilizando los avisos de seguridad publicados para los tipos de imagen de Docker que se listan en [Tipos de vulnerabilidades](#types). Por lo general, para que un paquete vulnerable pase la exploración, se necesita una versión posterior del paquete que incluya una corrección para la vulnerabilidad en cuestión. El mismo paquete puede tener varias vulnerabilidades; en tal caso, una única actualización del paquete puede corregir varias vulnerabilidades. Pulse el código de aviso de seguridad para ver más información sobre el paquete y para ver los pasos para actualizar el paquete.

   - Tabla **Problemas de configuración**: Muestra el ID de cada problema de configuración, el estado de la política correspondiente a dicho problema y la práctica de seguridad. Para ver más información para este problema, amplíe la fila. Se muestra un resumen del problema, que contiene un enlace con el aviso de seguridad correspondiente a dicho problema.
  
     La lista contiene sugerencias de acciones que puede emprender para aumentar la seguridad del contenedor y de cualquier valor de aplicación en el contenedor que se consideren no seguros. Amplíe la fila para ver cómo resolver el problema.

6. Realice la acción correctiva para cada problema que se muestra en el informe y vuelva a crear la imagen.

### Revisión de un informe de vulnerabilidad utilizando la CLI
{: #va_registry_cli}

Puede revisar la seguridad de imágenes de Docker que se han almacenado en sus espacios de nombres de {{site.data.keyword.registrylong_notm}} mediante la CLI.
{:shortdesc}

1. Enumere las imágenes en su cuenta {{site.data.keyword.cloud_notm}}. Se devolverá una lista de todas las imágenes, independientemente del espacio de nombres en el que se almacenan.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Compruebe el estado en la columna **ESTADO DE SEGURIDAD**.
    - `Sin problemas` No se han encontrado problemas de seguridad.
    - `<X> problemas` El número de posibles problemas o vulnerabilidades de seguridad que se han encontrado, donde `<X>` es el número de problemas.
    - `Explorando` La imagen se está explorando y no se ha determinado el estado de vulnerabilidad final.

3. Para ver los detalles para el estado, revise el informe de Vulnerability Advisor:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   En la salida de la CLI, puede ver la siguiente información sobre los problemas de configuración.
      - `Práctica de seguridad` Una descripción de la vulnerabilidad que se ha encontrado
      - `Acción de corrección` Detalles sobre cómo solucionar la vulnerabilidad

### Vulnerabilidades en paquetes en CentOS
{: #va_centos}

Si utiliza CentOS, es posible que obtenga falsos positivos en el informe, es decir, el informe podría advertir sobre una vulnerabilidad que no es tal. Esta situación se produce cuando Red Hat publica un aviso de seguridad pero el aviso de seguridad no se aplica a CentOS o el arreglo todavía no se ha portado a CentOS.
{:shortdesc}

Si recibe un informe que indica que su paquete tiene vulnerabilidades, siga los siguientes pasos:

1. Consulte los datos para actualizar el paquete pulsando el código del aviso de seguridad.
2. Actualice el paquete siguiendo los pasos para actualizar el paquete.
3. Si el paquete se actualiza, el resultado no era un falso positivo y ha finalizado la acción necesaria.
4. Si el paquete no se actualiza porque no hay disponible ninguna versión nueva para instalar, el resultado era un falso positivo. Puede añadir una política de exención para el aviso de seguridad; para ello, consulte [Establecimiento de políticas de exención organizativas](#va_managing_policy).

## Establecimiento de políticas de exención organizativas
{: #va_managing_policy}

Si desea gestionar la seguridad de una organización de {{site.data.keyword.cloud_notm}}, puede utilizar el valor de política para determinar si un problema está exento o no. Puede utilizar Container Image Security Enforcement para asegurarse de que el despliegue solo se permita desde imágenes que no tengan problemas de seguridad después de justificar los problemas exentos por su política.
{:shortdesc}

Puede desplegar contenedores desde cualquier imagen, independientemente del estado de seguridad, a menos que Container Image Security Enforcement esté desplegado en el clúster. Para saber cómo desplegar Container Image Security Enforcement, consulte [Instalación de la obligatoriedad de seguridad](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Al utilizar Container Image Security Enforcement, cualquier problema de seguridad detectado por Vulnerability Advisor impide a un contenedor desplegarse desde la imagen. Para permitir que se despliegue una imagen con problemas detectados, se deben añadir exenciones a su política.

### Establecimiento de políticas de exención organizativas utilizando la GUI
{: #va_managing_policy_gui}

Si desea establecer exenciones a la política utilizando la GUI, realice los pasos siguientes:

1. Inicie una sesión en {{site.data.keyword.cloud_notm}}. Debe haber iniciado la sesión para ver Vulnerability Advisor en la GUI.
2. Pulse el icono de **Menú de navegación** y pulse **Kubernetes**.
3. En **Vulnerability Advisor**, pulse **Valores de política**.
4. Pulse **Crear exención**.
5. Seleccione el tipo de problema.
6. Especifique el ID del problema.

   Puede encontrar esta información en el [informe de vulnerabilidad](#va_reviewing). La columna **ID de vulnerabilidad** contiene el ID que se utilizará para CVE o para los problemas de aviso de seguridad; la columna **ID de problema de configuración** contiene el ID que se utilizará para problemas de configuración.
   {: tip}

7. Seleccione el espacio de nombres de registro, el repositorio y la etiqueta a la que desee aplicar la exención.
8. Pulse **Guardar**.

También puede editar y eliminar exenciones pasando el ratón por encima de la fila relevante y pulsando el icono **abrir y cerrar la lista de opciones**.

### Establecimiento de políticas de exención organizativas utilizando la CLI
{: #va_managing_policy_cli}

Si desea establecer exenciones a la política utilizando la CLI, puede ejecutar los mandatos siguientes:

- Para crear una exención para un problema de seguridad, ejecute el mandato [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add).
- Para listar las exenciones para problemas de seguridad, ejecute el mandato [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list).
- Para listar los tipos de problemas de seguridad que puede eximir, ejecute el mandato [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types).
- Para suprimir una exención para un problema de seguridad, ejecute el mandato [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm).

Para obtener más información sobre los mandatos, puede utilizar el distintivo `--help` al ejecutar el mandato.
