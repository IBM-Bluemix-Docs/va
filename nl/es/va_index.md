---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-31"

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

Vulnerability Advisor comprueba el estado de seguridad de las imágenes de contenedor proporcionadas por IBM, terceros, o que se han añadido al espacio de nombres del registro de la organización.
{:shortdesc}


Cuando añade una imagen a un espacio de nombres, Vulnerability Advisor la explora automáticamente para detectar problemas de seguridad y potenciales vulnerabilidades. Si encuentra problemas de seguridad, se proporcionan instrucciones para poder arreglar el problema de vulnerabilidad notificado. Puede seguir desplegando contenedores a partir de imágenes vulnerables, pero tenga en cuenta que dichos contenedores pueden ser atacados o estar en peligro.


## Acerca de Vulnerability Advisor
{: #about}

Vulnerability Advisor proporciona funciones de gestión de la seguridad para {{site.data.keyword.containerlong}}. Vulnerability Advisor genera un informe de estado de la seguridad, sugiere arreglos y prácticas recomendadas y ofrece funciones de gestión para restringir la ejecución de las imágenes que no son seguras. Arreglar los problemas de seguridad y configuración de los que informa Vulnerability Advisor puede ayudarle a proteger la infraestructura de {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Vulnerability Advisor incluye las siguientes características:

-   Explora imágenes para detectar vulnerabilidades
-   Proporciona un informe de evaluación basado en prácticas de seguridad específicas de {{site.data.keyword.containerlong_notm}}
-   Ofrece recomendaciones para proteger los archivos de configuración de un subconjunto de tipos de aplicación
-   Proporciona instrucciones sobre cómo solucionar un [paquete vulnerable](#packages) o un [problema de configuración](#app_configurations) notificado en sus informes

En el panel de control Registro, la columna **INFORME DE SEGURIDAD** muestra el estado de sus repositorios.El informe identifica prácticas recomendadas de seguridad en la nube para sus imágenes.  

El panel de control de Vulnerability Advisor muestra una visión general y una evaluación de la seguridad para una imagen.Para obtener más información sobre el panel de control de Vulnerability Advisor, consulte [Revisión de un informe de vulnerabilidad](#va_reviewing).
	
	
**Protección de los datos**

Para escanear imágenes y contenedores en su cuenta por motivos de seguridad, Vulnerability Advisor recopila, almacena y procesa la información siguiente:
- campos de texto de formato libre, incluidos ID, descripciones y nombres de imágenes (etiqueta de registro, de espacio de nombres, de nombre de repositorio y de imagen)
- metadatos de Kubernetes, incluidos nombres de recursos de Kubernetes como por ejemplo pod, conjunto de réplicas y nombres de despliegue
- metadatos sobre las modalidades de archivos y la creación de indicaciones de fecha y hora de los archivos de configuración
- el contenido del sistema y de los archivos de configuración de aplicación en imágenes y contenedores
- paquetes y bibliotecas instalados (incluidas sus versiones)

No coloque información personal en ningún campo ni ubicación que procese Vulnerability Advisor, tal como se identifica en la lista anterior.

Los resultados del escaneo, agregados en un nivel de centro de datos, se procesan para crear métricas anonimizadas para operar y mejorar el servicio.

Los resultados del escaneo se suprimen a los 30 días una vez generados.



## Tipos de vulnerabilidades
{: #types}

### Paquetes vulnerables
{: #packages}

Vulnerability Advisor comprueba si hay paquetes vulnerables en imágenes basadas en sistemas operativos soportados y proporciona un enlace con los avisos de seguridad relevantes acerca de la vulnerabilidad. {:shortdesc}

Se muestran los paquetes con problemas de vulnerabilidad conocidos en los resultados del escaneo. Las posibles vulnerabilidades se actualizan a diario a partir de avisos de seguridad publicados para los tipos de imagen de Docker que se muestran en la tabla siguiente. Por lo general, para que un paquete vulnerable pase la exploración, se necesita una versión posterior del paquete que incluya una corrección para la vulnerabilidad en cuestión. El mismo paquete puede tener varias vulnerabilidades; en tal caso, una única actualización del paquete puede corregir varias vulnerabilidades. 


  |Imagen base de Docker|Origen de los avisos de seguridad|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://git.alpinelinux.org/) y [CIRCL CVE Search ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.circl.lu/)|
  |CentOS| [Archivos de anuncios de CentOS ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-announce/) y [Archivos de anuncios de CentOS CR ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Anuncios de seguridad de Debian ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Avisos de seguridad de Ubuntu ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/)|
  {: caption="Tabla 1. Imágenes base de Docker soportadas que Vulnerability Advisor comprueba en busca de paquetes vulnerables" caption-side="top"}




### Problemas de configuración
{: #app_configurations}

Los problemas de configuración son problemas de seguridad potenciales relacionados con cómo se configura una app. Muchos de los problemas notificados pueden arreglarse actualizando Dockerfile.
{:shortdesc}

Las imágenes solo se exploran si se basan en un sistema operativo que recibe soporte de Vulnerability Advisor. Vulnerability Advisor comprueba los valores de configuración para los siguientes tipos de apps:
-   MySQL
-   NGINX
-   Apache



## Instalación del escáner de contenedores
{: #va_install_livescan}

Antes de empezar:

1.  Inicie una sesión en el cliente de CLI de {{site.data.keyword.Bluemix_notm}}. Si tiene una cuenta federada, utilice `--sso`.
2.  [Apunte la CLI de `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) al clúster donde desea utilizar una gráfica de Helm.
3.  Cree un ID de servicio y una clave de API para el escáner de contenedores y proporcióneles un nombre:
    1.  Cree un ID de servicio ejecutando el mandato siguiente, sustituyendo `<scanner_serviceID>` por un nombre de su elección para el ID de servicio. Anote su **CRN**.
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Cree una clave de API de servicio, donde `<scanner_serviceID>` es el ID de servicio creado en el paso anterior y sustituyendo `<scanner_APIkey_name>` por un nombre de su elección para la clave de API de escáner.

        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    Se devuelve la clave de API del escáner.
	
	    Asegúrese de almacenar la clave de API del escáner de forma segura porque no se puede recuperar más tarde.
	    {: tip}
	
    3.  Cree una política de servicios que otorgue el rol `Writer`.
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Para configurar la gráfica de Helm:

1.  [Configure Helm en el clúster](../../containers/cs_integrations.html#helm). Si utiliza una política de RBAC para otorgar acceso al tiller de Helm, asegúrese de que el rol del tiller tenga acceso a todos los espacios de nombres para que el escáner pueda ver contenedores en todos los espacios de nombres.
2.  Añada el repositorio de gráficas de IBM al Helm, como por ejemplo `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Guarde los valores de configuración predeterminados para la gráfica de Helm del escáner del contenedor en un archivo YAML local. Incluya el repositorio de gráficas, como por ejemplo `ibm-incubator`, en la vía de acceso de gráficas de Helm.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Edite el archivo `config.yaml`.

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
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th>Campo</th>
    <th>Valor </th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Especifique el URL de punto final regional de Vulnerability Advisor. Para obtener el URL, ejecute <code>bx cr info</code> y recupere la dirección <strong>Registro de contenedores</strong>. Sustituya <code>registry</code> por <code>va</code>. Por ejemplo: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Sustituya por el ID de cuenta de {{site.data.keyword.Bluemix_notm}} en el que se encuentre el clúster. Para obtener el ID de cuenta, ejecute <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Sustituya por el clúster de Kubernetes en el que desee instalar el escáner de contenedores. Para listar ID de clúster, ejecute <code>bx cs clusters</code>.</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Sustituya por la clave de API de escáner que ha creado anteriormente.</td>
    </tr>
    </tbody></table>

5.  Instale la gráfica de Helm en el clúster con el archivo `config.yaml` actualizado. Las propiedades actualizadas se almacenan en un mapa de configuración para su gráfica. Sustituya `<myscanner>` por un nombre de su elección para la gráfica de Helm. Incluya el repositorio de gráficas, como por ejemplo `ibm-incubator`, en la vía de acceso de gráficas de Helm.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **Nota**: El escáner de contenedores está instalado en el espacio de nombres `kube-system`, pero escanea contenedores desde todos los espacios de nombres.
6.  Compruebe el estado de despliegue del diagrama. Cuando la gráfica esté lista, el campo **ESTADO** junto a la parte superior de la salida tendrá un valor de `DEPLOYED`.
    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Una vez que se despliegue la gráfica, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner ahora está instalado, y el agente de livescan se despliega como un [DaemonSet ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en el clúster. Aunque el escáner se despliega en el espacio de nombres de `kube-system`, escanea todos los contenedores asignados a los pods en todos los espacios de nombres de Kubernetes, como por ejemplo `default`.



## Consulta de un informe de vulnerabilidad
{: #va_reviewing}

Antes de desplegar una imagen, puede consultar el informe del Vulnerability Advisor para obtener detalles sobre los paquetes vulnerables y los valores de app no seguros.
{:shortdesc}



1.  Inicie una sesión en {{site.data.keyword.Bluemix_notm}}. 2.  Pulse **Catálogo**.
3.  En **Infraestructura**, pulse **Contenedores**.
4.  Pulse el mosaico **Container Registry**.
5.  Expanda **Vulnerability Advisor** y pulse **Repositorios escaneados**.
6.  Para ver el informe de la imagen con la etiqueta `latest`, pulse la fila correspondiente a ese repositorio. El informe muestra el número de problemas y si son paquetes vulnerables o problemas de configuración. Si no existe la etiqueta `latest` en el repositorio, se utilizará la imagen más reciente.
7.  Para ver la información sobre cada paquete vulnerable para la imagen que ha seleccionado, en la tabla **Paquetes vulnerables encontrados**, pulse el enlace de la columna **VULNERABILIDADES** para abrir el informe.
    1.  Para ver más información, amplíe el resumen. 2.  Si se ha proporcionado un aviso del distribuidor de un sistema operativo, pulse el enlace de la columna **AVISO OFICIAL**.
8.  Para ver la información sobre cada problema de configuración, en la tabla **Problemas de configuración encontrados**, pulse el enlace de la fila correspondiente al problema.
9.  Realice la acción correctiva para cada problema que se muestra en el informe y vuelva a crear la imagen. Algunos problemas del Dockerfile se pueden resolver usando el código suministrado en [Resolución de problemas en imágenes](#va_report).

Si hay vulnerabilidades y no las corrige, los problemas pueden afectar a la seguridad de los contenedores creados con dicha imagen. Sin embargo, puede seguir utilizando una imagen que tenga problemas de seguridad y de configuración en un contenedor.





## Revisión de un informe de vulnerabilidades mediante la CLI
{: #va_registry_cli}

Puede revisar la seguridad de imágenes de Docker que se han almacenado en sus espacios de nombres de {{site.data.keyword.registrylong_notm}} mediante la CLI.
{:shortdesc}

1.  Enumere las imágenes en su cuenta {{site.data.keyword.Bluemix_notm}}. Se devolverá una lista de todas las imágenes, independientemente del espacio de nombres en el que se almacenan.

    ```
        bx cr image-list
    ```
    {: pre}

2.  Compruebe el estado en la columna **ESTADO DE SEGURIDAD**.
    -   `No Issues`: No se han encontrado problemas de seguridad.
    -   `X Issues`: Se han encontrado problemas o vulnerabilidades de seguridad potenciales.
    -   `Scanning`: La imagen se está escaneando y el estado de vulnerabilidad final no está determinado aún.
4.  Para ver los detalles para el estado, revise el informe de Vulnerability Advisor.

    ```
        bx cr va registry.<región>/<mi_espaciodenombres>/<mi_imagen>:<etiqueta>
    ```
    {: pre}

    En la salida de la CLI, puede ver la siguiente información sobre los problemas de configuración.
      - Práctica de seguridad: Una descripción de la vulnerabilidad que se ha encontrado
      - Acción de corrección: Detalles sobre cómo solucionar la vulnerabilidad


## Resolución de problemas comunes de las imágenes
{: #va_report}

Revise los arreglos de muestra para problemas comunes de los que puede haber informado Vulnerability Advisor. Algunos problemas pueden arreglarse actualizando Dockerfile.
{:shortdesc}

### Antigüedad máxima de la contraseña, días mínimos para la contraseña y longitud mínima de la contraseña
{: #va_password}

**Problema**: Recibe una o varias de las vulnerabilidades siguientes:

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

**Arreglo**: Establezca la conformidad de las contraseñas añadiendo a su Dockerfile el código siguiente.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}

### Vulnerabilidad de SSH
{: #ssh}

**Problema**: Se devuelve la siguiente vulnerabilidad:

```
No debe estar instalado el servidor de SSH.
```
{: screen}

**Arreglo**: En lugar de utilizar SSH, utilice `docker attach` o `docker exec` para acceder a su contenedor. Asegúrese de que el Dockerfile no contiene ningún paso para la instalación de un servidor de SSH.
