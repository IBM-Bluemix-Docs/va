---

copyright:
  years: 2015, 2017

  lastupdated: "2017-03-07"

  ---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Imagen de contenedor de IBM MQ Bluemix
{: #ibm_mq}

La imagen ibm-mq se proporciona para {{site.data.keyword.containershort}}. Incluye {{site.data.keyword.IBM}} MQ Advanced for Developers (sin garantía). 

{:shortdesc}


## Funcionamiento
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers contiene todo lo que necesita para empezar a desarrollar sus propias soluciones de mensajería y aplicaciones {{site.data.keyword.IBM_notm}} MQ. 

Para desarrollar soluciones de mensajería y aplicaciones {{site.data.keyword.IBM_notm}} en {{site.data.keyword.Bluemix_notm}}, puede desplegar la imagen de Docker ibm-mq en {{site.data.keyword.containershort}}. A continuación, puede crear y ejecutar gestores de colas y utilizar la IU web o un terminal para configurarlos de acuerdo con los requisitos de su solución de mensajería o de su aplicación.

**Nota:** Puede utilizar la imagen ibm-mq solamente para el desarrollo y la prueba de unidad. También puede utilizar la imagen para explorar el producto, aprender de guías de aprendizaje y evaluar la contribución que {{site.data.keyword.IBM_notm}} MQ puede realizar en la organización.

## Qué incluye
{: #ibm_mq_what}

Esta imagen ibm-mq contiene el paquete de software para {{site.data.keyword.IBM_notm}} MQ Advanced for Developers Versión 9.0.x, que es una versión con todas las funciones del producto que puede utilizar para el desarrollo y la prueba de unidad. Puede descargar esta versión de forma gratuita y puede utilizarla dentro de los términos de la licencia.

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers está disponible en sistemas operativos Windows de 64 bits y sistemas operativos Linux on x86-64. Todos los requisitos previos del producto se incluyen en el paquete de descarga. Para obtener más información, consulte [Descargas de IBM MQ ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}.

Para obtener información sobre las características que se incluyen en {{site.data.keyword.IBM_notm}} MQ Advanced for Developers, consulte [IBM MQ ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}.



## Restricciones de uso
{: #ibm_mq_usage}

En la siguiente tabla se muestran las restricciones de uso que se aplican al uso gratuito de la imagen ibm-mq en {{site.data.keyword.Bluemix_notm}}:

| Entorno	| Restricciones para el uso gratuito |
|-------------|-------------------------|
| Desarrollo | Uso gratuito ilimitado de la imagen ibm-mq para crear contenedores únicos para el desarrollo. |
| Prueba | Uso gratuito ilimitado de la imagen ibm-mq para crear contenedores únicos para pruebas de unidad. Consulte la [Información de licencia](getstarted.html#ibm_mq_license ) para obtener más información sobre qué pruebas se permiten. |
| Producción | La licencia de {{site.data.keyword.IBM_notm}} MQ Advanced for Developer no permite su uso para producción. |

**Nota:** El precio de la imagen ibm-mq es independiente del precio de los contenedores que utiliza en {{site.data.keyword.Bluemix_notm}}.


## Información sobre la licencia
{: #ibm_mq_license}

Información sobre licencias:

*	Los Dockerfiles y scripts asociados están bajo licencia [Apache License 2.0 ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}.
* [Información de la licencia para {{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}.

**Nota**: La licencia no permite más distribución. Los términos para {{site.data.keyword.IBM_notm}} MQ en la imagen restringen el uso para el desarrollo y las pruebas de unidad.


## Iniciación
{: #ibm_mq_get_started}

Realice los siguientes pasos para ejecutar y gestionar {{site.data.keyword.IBM_notm}} MQ en un contenedor que se ejecuta en {{site.data.keyword.Bluemix_notm}}:

1. [Suministrar un contenedor basado en la imagen ibm-mq](ibm_mq.html#ibm_mq_provision)

    **Nota:** El contenedor está vinculado al estado de ejecución del Gestor de colas que incluye en su interior. Si detiene el gestor de colas utilizando la consola o la línea de mandatos, el contenedor se detendrá poco después. 

2. Gestionar recursos de {{site.data.keyword.IBM_notm}} MQ que están ejecutándose en un contenedor. Puede hacerlo gráficamente desde la consola web de IBM MQ o mediante programación utilizando mandatos desde un terminal. 

    1. Elija cualquiera de las siguientes opciones para gestionar recursos de {{site.data.keyword.IBM_notm}} MQ en el contenedor:
    
        *	Inicie la consola web de {{site.data.keyword.IBM_notm}} MQ y gestione {{site.data.keyword.IBM_notm}} MQ gráficamente. Para obtener más información, consulte [Inicio de la consola web de IBM MQ](ibm_mq.html#ibm_mq_webui).
        *	Desde un terminal, utilice la CLI de Docker. Para obtener más información, consulte [Configuración de un terminal para que utilice la CLI de Docker](ibm_mq.html#ibm_mq_docker_commands).
        * Desde un terminal, utilice la CLI de IBM Bluemix Container Service. Para obtener más información, consulte [Configuración de un terminal para que utilice la CLI de IBM Bluemix Container Service](ibm_mq.html#ibm_mq_containers_cli).
        
    2. Configure el Gestor de colas de {{site.data.keyword.IBM_notm}} MQ que se ejecuta en el contenedor. 

        * Para gestionar el Gestor de colas mediante la línea de mandatos, utilice el programa *runmqsc*. Ejecute el siguiente mandato: `runmqsc nombre_GC` donde *nombre_GC* es el nombre del gestor de colas. Para obtener más información, consulte [Gestión del contenedor mediante la línea de mandatos](ibm_mq.html#ibm_mq_access).
    
        * Para gestionar el gestor de colas de forma gráfica, utilice la interfaz web.
        
    3. Siga las instrucciones de [IBM MQ Knowledge Center ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} para crear las colas de mensajes en el gestor de colas y transferir u obtener mensajes entre ellas.
    
        Por ejemplo, ejecute el mandato siguiente en `runmqsc` para crear una nueva cola local: `DEFINE QLOCAL ('MY.Q')`, donde *MY.Q* es el nombre de la cola local del terminal. Puede ver la cola en la consola web o puede mostrar la cola y sus propiedades ejecutando siguiente mandato en 'runmqsc`: `DISPLAY QLOCAL('MY.Q')`.
    
3. [Opcional] Supervise los registros de {{site.data.keyword.IBM_notm}} MQ dentro del contenedor. Para obtener más información, consulte [Gestión del contenedor mediante la línea de mandatos](ibm_mq.html#ibm_mq_access).

Para obtener más información sobre la guía de iniciación a IBM MQ, consulte la [página de bienvenida de IBM MQ Versión 9.0.x ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}

Para obtener más información sobre la imagen y cómo crear su propia imagen localmente en Docker, consulte la [página de GitHub de MQ Docker ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/mq-docker){: new_window}
    
Para ver la información y los artículos más recientes del blog sobre IBM MQ, consulte el [blog de MQ Developerworks ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}


### Suministro de un contenedor basado en la imagen ibm-mq
{: #ibm_mq_provision}

Siga los siguientes pasos para suministrar un contenedor Docker en {{site.data.keyword.Bluemix_notm}} basado en la imagen ibm-mq proporcionada por {{site.data.keyword.IBM_notm}}:

1.	Inicie una sesión en {{site.data.keyword.Bluemix_notm}}. Inicie sesión con su ID de {{site.data.keyword.Bluemix_notm}}.
2.	En el catálogo, seleccione **Contenedores** y seleccione la imagen *ibm-mq*.
3.	Seleccione **Único** para crear un contenedor de instancia única, que puede utilizarse para fines de desarrollo y pruebas.
4.	Especifique el nombre del contenedor; por ejemplo, mq.
5.	Seleccione el tamaño del contenedor.
6.	En el campo Dirección IP pública, seleccione **Solicitar y enlazar IP pública**.
7.	En el campo Puertos públicos, especifique **1414/tcp, 9443/tcp**.
8.	Expanda las opciones avanzadas, pulse **Añadir una nueva variable de entorno** y escriba las siguientes variables de entorno: LICENSE y MQ_QMGR_NAME.

    * Establezca el valor de **LICENSE** en **accept** para aceptar los términos de la licencia de IBM MQ Advanced for Developers. 
    
        Si desea ver la licencia antes de aceptarla, puede establecer LICENSE en “view” y la licencia se imprimirá en los registros del contenedor. 
    
        Si el inglés no es su primer idioma, puede ver el archivo de licencia en un idioma distinto estableciendo otra variable de entorno denominada **LANG** en uno de los siguientes valores: *zh_TW* para chino tradicional, *zh* para chino, *cs* para checo, *en* para inglés, *fr* para francés, *de* para alemán, *el* para griego, *id* para indonesio, *it* para italiano, *ja* para japonés, *ko* para coreano, *lt* para lituano, *pl* para polaco, *pt* para portugués, *ru* para ruso, *sl* para esloveno, *es* para español o *tr* para turco.

    * Establezca el valor de **MQ_QMGR_NAME** en el valor *MYMQ*, donde *MYMQ* es el nombre que ha elegido para su gestor de colas.
 
        **Nota**: Si no especifica valores para las variables de entorno LICENSE y MQ_QMGR_NAME, el contenedor se crea pero no se inicia. 
    
    * [Opcional] De forma predeterminada, la consola web de {{site.data.keyword.IBM_notm}} se inicia en el contenedor. Para cambiar el comportamiento predeterminado, establezca el valor de la variable de entorno **MQ_DISABLE_WEB_CONSOLE** en *true*.
    
        **Nota:** Para utilizar la consola web, no establezca la variable de entorno *MQ_DISABLE_WEB_CONSOLE*.

    * [Opcional] Para ver los registros de MQ a través de la IU de {{site.data.keyword.Bluemix_notm}}, establezca la variable de entorno **LOG_LOCATIONS** en el valor `/var/mqm/qmgr/nombre_GC/errors/AMQERR01.LOG` donde *nombre_GC* es el valor que ha establecido para la variable de entorno *MQ_QMGR_NAME*.
    
    *	[Opcional] De forma predeterminada, se crean los objetos predeterminados de {{site.data.keyword.IBM_notm}} MQ Developer. Para cambiar el comportamiento predeterminado, Para cambiar el comportamiento predeterminado, establezca la variable de entorno **MQ_DEV** en *false* y modifique los valores de instalación predeterminados para IBM MQ con sus valores personalizados. Para obtener más información, consulte [Modificación de los valores de instalación predeterminados](ibm_mq.html#ibm_mq_dev_default)
 
9. [Opcional] En la sección Opciones avanzadas, asigne un volumen. Utilice volúmenes de {{site.data.keyword.Bluemix_notm}} para almacenar datos de {{site.data.keyword.IBM_notm}} MQ y la configuración de forma permanente entre migraciones de contenedor. Los datos de {{site.data.keyword.IBM_notm}} MQ que no se almacenan en un volumen se perderán si se finaliza un contenedor.

    La imagen ibm-mq puede utilizarse con volúmenes de {{site.data.keyword.Bluemix_notm}}, sin embargo los volúmenes deben estar montados en **/mnt/mqm** para poder ser detectados y utilizados por {{site.data.keyword.IBM_notm}} MQ.

    De forma predeterminada, todos los contenedores creados sin un volumen perderán todos los datos de IBM MQ al detener el contenedor. Si desea evitarlo, debe utilizar la mensajería persistente y volúmenes de {{site.data.keyword.Bluemix_notm}}.

    1. Pulse **Crear un volumen**, especifique un nombre de volumen y seleccione una compartición de archivos.
    2. Pulse **Asignar un volumen existente** y seleccione el volumen que ha creado. Escriba **/mnt/mqm** en el recuadro de vía de acceso. Asegúrese de que el recuadro **Sólo lectura** no esté seleccionado.

    **Nota:** Al montar un volumen para utilizarlo como directorio de datos de IBM MQ, debe montarlo en la ubicación */mnt/mqm*. Montándolo en */mnt/mqm*, el script ibm-mq detectará el volumen y configurará el sistema de archivos de IBM MQ para que lo utilice.
     
10.	Pulse **Crear** y espere a que el contenedor empiece a ejecutarse.

    Una vez creado el contenedor, se abre el panel de control del contenedor. Compruebe que el estado del contenedor sea **En ejecución**. 

    Para consultar los registros de su contenedor, pulse **Supervisión y registros** y seleccione el separador **Registro**. Se muestran los registros. Para el análisis avanzado de los registros, pulse **Vista avanzada** para visualizar sus registros en Kibana.

Ha creado e iniciado un gestor de colas. Ahora puede conectar una aplicación MQ con los siguientes detalles de conexión:

| Propiedad de conexión | Valor |
|-------|-------|
| Dirección IP  | Dirección IP pública del contenedor |
| Puerto | 1414 |
| Canal | DEV.APP.SVRCONN |


### Gestión del contenedor mediante la línea de mandatos
{: #ibm_mq_access}

Después de desplegar el contenedor que ejecuta {{site.data.keyword.IBM_notm}} MQ en {{site.data.keyword.Bluemix_notm}}, puede acceder al contenedor y al estado del contenedor y validar la instalación de {{site.data.keyword.IBM_notm}} MQ.

Complete los siguientes pasos para verificar la instalación y configuración de {{site.data.keyword.IBM_notm}} MQ en el contenedor:

1.	Configure la CLI de IBM Containers. Para obtener más información, consulte la [página de CLI de Bluemix](https://plugins.ng.bluemix.net/ui/home.html) y siga las instrucciones para instalar el plugin de Cloud Foundry Container Service más reciente.

2.	Desde un terminal, inicie la sesión en su organización y espacio de {{site.data.keyword.Bluemix_notm}} donde el contenedor se está ejecutando con su ID de {{site.data.keyword.Bluemix_notm}}. Ejecute los siguientes mandatos:

    1. `bx login`
    2. `bx ic init`
    
3.	Compruebe el estado del contenedor. Ejecute el siguiente mandato: `bx ic ps`

4.	Adjunte una sesión Bash al contenedor:

    `bx ic exec -it nombre_contenedor /bin/bash`
    
    donde nombre_contenedor es el nombre del contenedor.
    
5.	Ejecute los mandatos para gestionar la configuración de MQ, por ejemplo:

| Mandato | Acción |
|---------|---------|
| dspmqver | Verificar la versión de IBM MQ e información de compilación. |
| dspmq | Comprobar el estado del gestor de colas que se está ejecutando en el contenedor. | 
| runmqsc | Gestionar recursos de MQ. |

Para supervisar los registros de MQ, seleccione una de las siguientes opciones:

* Si el terminal se ha configurado para ejecutar mandatos de CLI de {{site.data.keyword.containershort}}, ejecute el mandato siguiente: `bx ic exec id_contenedor tail -f /var/mqm/qmgr/nombre_GC/errors/AMQERR01.LOG` donde *id_contenedor* es el nombre del contenedor y *nombre_GC* es el nombre del gestor de colas.
    
* Si el terminal se ha configurado para ejecutar mandatos de CLI de Docker, ejecute el mandato siguiente: `docker exec id_contenedor tail -f /var/mqm/qmgr/nombre_GC/errors/AMQERR01.LOG` donde *id_contenedor* es el nombre del contenedor y *nombre_GC* es el nombre del gestor de colas.
    
    

### Inicio de la consola web de IBM MQ
{: #ibm_mq_webui}

Siga los siguientes pasos para iniciar la consola web de {{site.data.keyword.IBM_notm}} MQ:

1.	En el panel de control de {{site.data.keyword.Bluemix_notm}}, seleccione el contenedor y compruebe que el estado del contenedor esté establecido en *En ejecución*.
2.	Compruebe que el contenedor tenga configurado el puerto público 9443.
3.	Consulte los detalles de contenedor para buscar la IP pública o solicite y enlace una si todavía no lo ha hecho.
4.	Desde un navegador web, inicie el URL siguiente: `https://IPPúblicaContenedorDocker:9443/ibmmq/console/` donde IPPúblicaContenedorDocker es la IP pública de su contenedor.

    Se abre un aviso de seguridad porque el certificado TLS que utiliza el servidor web al alojar la consola MQ es autofirmado. Pulse **Añadir excepción** y confirme.

    **Nota:** En un entorno de producción, utilice su propio certificado que el navegador reconozca y confíe en él. Este certificado debe ser emitido por una entidad emisora de certificados. 
    
    El navegador abre la interfaz de usuario web.
    
5. Inicie sesión en la consola de {{site.data.keyword.IBM_notm}} MQ con las credenciales:

    1.	Usuario: admin
    2.	Contraseña: passw0rd
    
Ahora está listo para administrar IBM MQ.

### Configuración de un terminal para que utilice la CLI de IBM Bluemix Container Service
{: #ibm_mq_containers_cli}

Utilice la CLI de {{site.data.keyword.containershort}} para ejecutar los mandatos de administración de {{site.data.keyword.IBM_notm}} MQ directamente en un contenedor.

Complete los siguientes pasos para configurar un terminal para ejecutar mandatos bx ic para gestionar IBM MQ:

1.	Configure la CLI de {{site.data.keyword.containershort}}. Para obtener más información, consulte la [página de CLI de Bluemix](https://plugins.ng.bluemix.net/ui/home.html) y siga las instrucciones para instalar el plugin de Cloud Foundry Container Service más reciente.

2.	En un terminal, inicie la sesión en {{site.data.keyword.Bluemix_notm}}. Ejecute los siguientes mandatos:

    1. `bx login`
    2. `bx ic init`
    
3.	Adjunte una sesión Bash al contenedor:

    `bx ic exec -it nombre_contenedor /bin/bash`
    
    donde nombre_contenedor es el nombre del contenedor.
    


### Configuración de un terminal para que utilice la CLI de Docker
{: #ibm_mq_docker_commands}

Utilice la CLI de Docker para ejecutar los mandatos de administración de {{site.data.keyword.IBM_notm}} MQ directamente en un contenedor.

Complete los siguientes pasos para configurar un terminal para ejecutar mandatos de Docker para gestionar IBM MQ:

1.	Configure la CLI de {{site.data.keyword.containershort}}. Para obtener más información, consulte la [página de CLI de Bluemix](https://plugins.ng.bluemix.net/ui/home.html) y siga las instrucciones para instalar el plugin de Cloud Foundry Container Service más reciente.

2.	En un terminal, inicie la sesión en {{site.data.keyword.Bluemix_notm}}. Ejecute los siguientes mandatos:

    1. `bx login`
    2. `bx ic init`
    3. Copie los valores que se proporcionan para las siguientes variables de entorno: DOCKER_HOST, DOCKER_CERT_PATH y DOCKER_TLS_VERIFY.
    4. Altere temporalmente el entorno de Docker local estableciendo las siguientes variables para conectarse a {{site.data.keyword.containershort}}:
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Adjunte una sesión Bash al contenedor:

    `bx ic exec -it nombre_contenedor /bin/bash`
    
    donde nombre_contenedor es el nombre del contenedor.
    
**Nota:** Esta opción solamente da soporte a algunos mandatos de Docker.




### Modificación de los valores de instalación predeterminados
{: #ibm_mq_dev_default}

Cuando el gestor de colas se crea, se creará con los valores predeterminados de {{site.data.keyword.IBM_notm}} MQ Developer. Estos objetos predeterminados están diseñados para ayudarle a empezar rápidamente a desarrollar aplicaciones o incluso a probar {{site.data.keyword.IBM_notm}} MQ. 

El gestor de colas se creará con los siguientes objetos predeterminados:

* Un usuario Administrador de MQ **admin** con la contraseña *passw0rd*.
*	Un usuario Aplicación de MQ **app** sin contraseña.
*	Un escucha **DEV.LISTENER.TCP** configurado para enlazar con el puerto 1414/TCP.
*	Un canal **DEV.APP.SVRCONN** configurado para manejar las conexiones de aplicación de cliente de MQ.
*	Un canal **DEV.ADMIN.SVRCONN** configurado para manejar las conexiones de administración de MQ.
*	3 colas **DEV.QUEUE.1**, **DEV.QUEUE.2**, **DEV.QUEUE.3** donde puede colocar mensajes.
*	Una cola de mensajes no entregados **DEV.DEAD.LETTER.QUEUE** donde se colocarán los mensajes no entregados.
*	Un tema base **DEV.BASE.TOPIC** con la serie de tema */dev*.
*	Un registro de autenticación de canal para bloquear todas las conexiones de cliente a menos que se conecten a través del canal **DEV.APP.SVRCONN** o **DEV.ADMIN.SVRCONN**.
*	Un registro de autenticación de canal para bloquear todos los usuarios admin que se conecten excepto mediante el canal **DEV.ADMIN.SVRCONN**.
*	Un registro de autenticación de canal para permitir solo los usuarios admin que se conecten al canal **DEV.ADMIN.SVRCONN**.
*	Conexión de autenticación establecida para solicitar que las aplicaciones de administración especifiquen credenciales válidas al conectarse y para adoptar ese ID de usuario para las comprobaciones de autorización.

Con estos objetos predeterminados, puede conectar una aplicación cliente que puede transferir y obtener mensajes de una cola, o suscribirse y publicar en el tema.

Si desea personalizar los valores predeterminados del desarrollador que se crean, puede añadir las siguientes variables de entorno cuando cree el contenedor desde la imagen ibm-mq.

| Variable de entorno | Objetivo | Valor pred. |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | Establezca esta variable para cambiar la contraseña predeterminada del administrador de MQ creado por otra de su elección. La contraseña debe tener al menos 8 caracteres. El nombre del usuario administrador es admin y es fijo. | passw0rd |
| MQ_APP_PASSWORD | Establezca esta variable para establecer la contraseña para el usuario de la aplicación. El nombre de usuario para el usuario de la aplicación es app y es fijo. Si establece esta variable de entorno, debe proporcionar el ID de usuario y la contraseña cuando conecte clientes MQ. <br> La contraseña debe tener al menos 8 caracteres. |  |
| MQ_TLS_KEYSTORE | Establezca esta variable de entorno en la ubicación de un almacén de claves PKCS#12 que contiene el certificado que desea que la consola web de MQ y el Gestor de colas de IBM MQ utilicen para las operaciones de TLS. <br> Cuando se establece esta variable de entorno, los canales de desarrollador creados se habilitarán para TLS utilizando el cifrado ‘TLS_RSA_WITH_AES_256_GCM_SHA384’. <br> **Nota:** Debe hacer que el archivo de almacén de claves esté disponible dentro del contenedor de {{site.data.keyword.Bluemix_notm}}. Para ello, puede montar un volumen que contenga su almacén de claves cuando cree el contenedor. |  |
| MQ_TLS_PASSPHRASE | Establezca esta variable de entorno para establecer la contraseña para el almacén de claves especificado en la variable de entorno MQ_TLS_KEYSTORE. |  |
| MQ_DEV | Establezca esta variable de entorno para controlar si se crean los objetos predeterminados de {{site.data.keyword.IBM_notm}} MQ Developer. | true |
| MQ_DISABLE_WEB_CONSOLE | Establezca esta variable de entorno para inhabilitar la configuración y el inicio de la consola web si desea ahorrar en el uso de CPU/RAM de los contenedores. |  |



