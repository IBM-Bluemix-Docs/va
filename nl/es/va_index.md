---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-11"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, container scanner, containers, security issues, configuration issues,

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

Vulnerability Advisor comprueba el estado de seguridad de las imágenes de contenedor proporcionadas por {{site.data.keyword.IBM}}, terceros, o que se han añadido al espacio de nombres del registro de la organización. Si Container Scanner (en desuso) está instalado en cada clúster, Vulnerability Advisor también comprueba el estado de los contenedores que se están ejecutando.
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
- Explora problemas en los contenedores que se están ejecutando si se instala un [Container Scanner](#va_install_container_scanner) en cada clúster (en desuso)
- Proporciona un informe de evaluación basado en prácticas de seguridad específicas de {{site.data.keyword.containerlong_notm}}
- Ofrece recomendaciones para proteger los archivos de configuración de un subconjunto de tipos de aplicación
- Proporciona instrucciones sobre cómo solucionar un [paquete vulnerable](#packages) o un [problema de configuración](#app_configurations) notificado en sus informes
- Proporciona un veredicto a [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Aplica exenciones a los informes de una cuenta, espacio de nombres o repositorio, o a nivel de etiqueta para saber qué problemas no se aplican a su caso
- Proporciona enlaces para los contenedores asociados en la vista de **Etiqueta** de la interfaz gráfica de usuario de {{site.data.keyword.registrylong_notm}}. Puede obtener una lista de contenedores en ejecución que están utilizando dicha imagen en un clúster donde tenga instalado Container Scanner (en desuso).

En el panel de control Registro, la columna **Estado de política** muestra el estado de sus repositorios. El informe enlazado identifica prácticas recomendadas de seguridad en la nube para sus imágenes.

El panel de control de Vulnerability Advisor muestra una visión general y una evaluación de la seguridad de una imagen y, si Container Scanner (en desuso) está instalado, enlaces a los contenedores que están en ejecución. Si desea obtener más información sobre el panel de control de Vulnerability Advisor, consulte [Revisión de un informe de vulnerabilidad](#va_reviewing).

**Protección de los datos**

Para explorar imágenes y contenedores en su cuenta por motivos de seguridad, Vulnerability Advisor recopila, almacena y procesa la información siguiente:

- Campos de formato libre, incluidos ID, descripciones y nombres de imágenes (etiqueta de registro, de espacio de nombres, de nombre de repositorio y de imagen)
- Metadatos de Kubernetes, que incluye nombres de recursos de Kubernetes como, por ejemplo, pod, ReplicaSet y nombres de despliegue
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

Si Container Scanner (en desuso) se despliega, después de desplegar la imagen, Vulnerability Advisor seguirá explorando en busca de problemas de seguridad y de configuración en el contenedor. Puede resolver los problemas encontrados siguiendo los pasos descritos en [Revisión de un informe de contenedor](#va_reviewing_container).

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

## Instalación de Container Scanner (en desuso)
{: #va_install_container_scanner}

Container Scanner está en desuso.
{: deprecated}

Container Scanner permite a Vulnerability Advisor informar sobre cualquier problema encontrado en contenedores en ejecución que no estén presentes en la imagen base del contenedor. Si no realiza modificaciones de tiempo de ejecución en el contenedor, entonces no necesita Container Scanner porque el informe de la imagen muestra los mismos problemas.
{:shortdesc}

Para comprobar el estado de seguridad de los contenedores activos que están en ejecución en el clúster, puede instalar Container Scanner. Para proteger su app, Container Scanner explora periódicamente los contenedores en ejecución para que pueda detectar y rectificar cualquier vulnerabilidad detectada.

Puede configurar Container Scanner para que supervise las vulnerabilidades en los contenedores asignados a los pods en todos los espacios de nombres de Kubernetes. Si se encuentran vulnerabilidades, debe corregir los problemas con la imagen y, a continuación, volver a desplegar su app. Container Scanner sólo da soporte a los contenedores creados a partir de imágenes que estén almacenadas en {{site.data.keyword.registrylong_notm}}.

Para utilizar Container Scanner, debe configurar los permisos y, a continuación, configurar una [gráfica de Helm ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.helm.sh/developing_charts) y asociarla al clúster en el que desea utilizarla.

### Configurar permisos del servicio para Container Scanner (en desuso)
{: #va_install_container_scanner_permissions}

Container Scanner está en desuso.
{: deprecated}

Container Scanner requiere que se configuren permisos para que el servicio pueda funcionar.
{:shortdesc}

Para configurar los permisos del servicio, realice los pasos siguientes:

1. Inicie una sesión en el cliente de CLI de {{site.data.keyword.cloud_notm}}. Si tiene una cuenta federada, utilice `--sso`.
2. [Apunte la CLI de `kubectl`](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) al clúster donde desea utilizar una gráfica de Helm.
3. Cree un ID de servicio y una clave de API para Container Scanner y proporcióneles un nombre:
    1. Para crear un ID de servicio, ejecute el siguiente mandato, donde `<scanner_serviceID>` es un nombre de su elección para el ID de servicio. Anote su **CRN**.

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. Cree una clave de API de servicio, donde `<scanner_serviceID>` es el ID de servicio que creó en el paso anterior y `<scanner_APIkey_name>` es un nombre de su elección para la clave de API del explorador.

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       Se devuelve la clave de API del explorador.

       Asegúrese de almacenar la clave de API del explorador de forma segura porque no se puede recuperar más tarde. Además, asegúrese de tener una clave de API del servicio para cada clúster en el que se instalará el explorador.
       {: important}

    3. Cree una política de servicios que otorgue el rol `Writer`.

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### Configurar la gráfica de Helm (en desuso)
{: #va_install_container_scanner_helm}

Container Scanner está en desuso.
{: deprecated}

Configure una gráfica de Helm y asóciela al clúster en el que desea utilizarla.
{:shortdesc}

Para configurar una gráfica de Helm, realice los pasos siguientes:

1. [Configurar Helm en IBM Cloud Kubernetes Service](/docs/containers?topic=containers-helm#helm). Si utiliza una política de control de acceso basado en roles (RBAC) para otorgar acceso a Tiller, asegúrese de que el rol de Tiller tenga acceso a todos los espacios de nombres. Otorgar al rol de Tiller acceso a todos los espacios de nombres garantiza que Container Scanner pueda ver los contenedores en todos los espacios de nombres.

2. Añada el repositorio de gráficas de IBM al Helm, como por ejemplo `ibm`.

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. Guarde los valores de configuración predeterminados para la gráfica de Helm de Container Scanner en un archivo YAML local. Incluya el repositorio de gráficas, como por ejemplo `ibm`, en la vía de acceso de gráficas de Helm.

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. Edite el archivo `config.yaml`.

   ```yaml
   EmitURL: <regional_emit_URL>
   AccountID: <IBM_Cloud_account_ID>
   ClusterID: <cluster_ID>
   APIKey: <scanner_APIkey>
   ...
   ```
   {: pre}

   <table>
   <col width="22%">
   <col width="78%">
   <caption>Tabla 2. Visión general de los componentes del archivo YAML</caption>
   <thead>
   <th>Campo</th>
   <th>Valor</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>Sustituya <code>&lt;regional_emit_URL&gt;</code> por el URL de punto final regional de Vulnerability Advisor. Para obtener el URL, ejecute <code>ibmcloud cr info</code> y recupere la dirección <strong>Registro de contenedores</strong>. Por ejemplo, <code>https<span comment="make the link not a link">://us.</span>icr.io</code>. Añada <code>/va</code> al final de esta dirección. Por ejemplo, <code>https<span comment="make the link not a link">://us.</span>icr.io/va</code>. Para obtener más información sobre regiones, consulte [Regiones locales](/docs/services/Registry?topic=registry-registry_overview#registry_regions_local).</td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>Sustituya <code>&lt;IBM_Cloud_account_ID&gt;</code> por el ID de cuenta de {{site.data.keyword.cloud_notm}} en el que se encuentre el clúster. Para obtener el ID de cuenta, ejecute <code>ibmcloud account list</code>.</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>Sustituya <code>&lt;cluster_ID&gt;</code> por el clúster de Kubernetes en el que desee instalar Container Scanner. Para listar ID de clúster, ejecute <code>ibmcloud ks clusters</code>. <br> **Sugerencia**: Utilice el ID del clúster y no el nombre.
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td>Sustituya <code>&lt;scanner_APIkey&gt;</code> por la clave de API del explorador que ha creado anteriormente.</td>
   </tr>
   </tbody></table>

5. Instale la gráfica de Helm en el clúster con el archivo `config.yaml` actualizado. Las propiedades actualizadas se almacenan en un mapa de configuración (ConfigMap) para su gráfica. Sustituya `<myscanner>` por un nombre de su elección para la gráfica de Helm. Incluya el repositorio de gráficas, como por ejemplo `ibm`, en la vía de acceso de gráficas de Helm.

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   Container Scanner está instalado en el espacio de nombres `kube-system`, pero explora contenedores de todos los espacios de nombres.
   {:tip}

6. Compruebe el estado de despliegue de la gráfica. Cuando la gráfica esté lista, el campo **ESTADO** tiene un valor de `DESPLEGADO`.

   ```
   helm status <myscanner>
   ```
   {: pre}

7. Una vez que se despliegue la gráfica, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

   ```
   helm get values <myscanner>
   ```
   {: pre}

Container Scanner ahora está instalado, y el agente se despliega como un [DaemonSet ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en su clúster. Aunque Container Scanner se despliega en el espacio de nombres de `kube-system`, explora todos los contenedores asignados a los pods en todos los espacios de nombres de Kubernetes, como por ejemplo `default`.

## Ejecución de Container Scanner desde detrás de un cortafuegos (en desuso)
{: #va_firewall}

Container Scanner está en desuso.
{: deprecated}

Si su cortafuegos bloquea conexiones salientes, debe configurarlo.
{:shortdesc}

Para configurar el cortafuegos para permitir que los nodos trabajadores accedan al Container Scanner a través del puerto TCP `443` en las direcciones IP, como se describe en el paso 3, [Cómo permitir al clúster acceder a recursos de infraestructura y otros servicios a través de un cortafuegos público](/docs/containers?topic=containers-firewall#firewall_outbound).

## Revisión de un informe de contenedor (en desuso)
{: #va_reviewing_container}

Container Scanner está en desuso.
{: deprecated}

En el panel de control, puede ver el estado de un contenedor para determinar si su seguridad cumple con la política de la organización. También puede consultar un informe de seguridad de contenedor, que muestra información detallada sobre paquetes vulnerables y valores de aplicación o contenedores no seguros y si el contenedor cumple con las políticas de la organización.
{:shortdesc}

Compruebe que los contenedores que están en ejecución en el espacio sigan cumpliendo con la política de la organización revisando el campo **Estado de política**. El estado se muestra como una de las siguientes condiciones:

- `Conforme con política` No se han encontrado problemas de seguridad ni de configuración.
- `No conforme con política` Vulnerability Advisor ha encontrado problemas potenciales de seguridad o de configuración que han provocado que el contenedor no cumpla con la política. Si la política de la organización permite el despliegue de imágenes vulnerables, la imagen podría desplegarse en el estado `Desplegar con cuidado`, y se enviará un aviso al usuario que la despliega.
- `Evaluación incompleta` La exploración no se ha completado. La exploración puede seguir en ejecución, o el sistema operativo de la instancia del contenedor puede no ser compatible.

Compruebe si el contenedor está protegido todo lo posible visualizando su informe de seguridad y actúe de acuerdo con los problemas de configuración o seguridad de los que se le informa siguiendo los siguientes pasos:

1. Seleccione el contenedor del que desea visualizar un informe:
    1. Pulse el icono de **Menú de navegación** y pulse **Kubernetes**.
    2. Pulse **Registro**, pulse el mosaico **Repositorios** y amplíe la fila del repositorio que desee.
    3. Seleccione la fila de la imagen que desee.
    4. Seleccione el separador **Contenedores asociados** y, a continuación, seleccione la fila del contenedor que desee. Se abre el informe de seguridad.
2. Revise las secciones para ver los problemas potenciales de seguridad y de configuración para cada paquete en la imagen:

    - **Vulnerabilidades** Muestra una lista de los paquetes que contienen problemas de vulnerabilidad conocidos. La lista se actualiza a diario utilizando los avisos de seguridad publicados para los tipos de imagen de Docker que se listan en [Tipos de vulnerabilidades](#types). Por lo general, para que un paquete vulnerable pase la exploración, se necesita una versión posterior del paquete que incluya una corrección para la vulnerabilidad en cuestión. El mismo paquete puede tener varias vulnerabilidades; en tal caso, una única actualización del paquete puede corregir varias vulnerabilidades. Pulse el código de aviso de seguridad para ver más información sobre el paquete y para ver los pasos para actualizar el paquete.

    - **Problemas de configuración** Muestra una lista de sugerencias de acciones que puede emprender para aumentar la seguridad del contenedor y de cualquier valor de aplicación en el contenedor que se consideren no seguros. Amplíe la fila para ver cómo resolver el problema.

   Para cada elemento mostrado, se suministran acciones de corrección o sugerencias.

3. Revise el estado de política para cada problema de seguridad. El estado de la política indica si se trata de una exención para el problema.

    - `Activo` Hay un problema que no es una exención. Este problema afectará al estado de la seguridad.
    - `Exento` Los valores de la política consideran este problema como una exención.
    - `Parcialmente exento` Este problema está asociado con más de un aviso de seguridad. No todos los avisos de seguridad están exentos.

4. Decida cómo actualizar el contenedor para que pueda resolver los problemas.

    Para solucionar problemas con la imagen del contenedor, debe suprimir la instancia antigua y volver a desplegarla, lo que significa perder datos dentro del contenedor existente. Asegúrese de comprender bien la arquitectura del contenedor para elegir el método apropiado para volver a desplegar el contenedor.
    {: important}

    **Ejemplo**

    - Si el contenedor está desacoplado de los datos que calcula, puede detener el contenedor y suprimirlo, realizar los cambios necesarios a la imagen, y volver a desplegarla, sin pérdida de datos.
    - Puede utilizar un servicio de {{site.data.keyword.cloud_notm}} como, por ejemplo, [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about), que le ayude con la actualización de la instancia de contenedor vulnerable.
    - En una arquitectura de microservicios, puede direccionar tráfico a otra instancia de contenedor mientras soluciona los problemas de seguridad o configuración y enviar por push la imagen nueva en un despliegue rojo-negro.

5. Si no puede corregir el problema ahora, puede marcarlo como una exención en los valores de política, para evitar que el problema bloquee el despliegue del contenedor. Para marcar el problema como exento, pulse el icono **abrir y cerrar lista de opciones** y pulse **Crear exención**, consulte [Establecimiento de políticas de exención de la organización](#va_managing_policy).

6. Corrija los problemas descritos en el informe de **Seguridad** y vuelva a recrear la imagen o redesplegar el contenedor de acuerdo con el método que elija.
