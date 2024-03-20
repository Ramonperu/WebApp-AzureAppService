

# Host a web application with Azure App Services

Bienvenidos a la guía para la creación de aplicaciones webs de Azure, este resumen practico esta hecho gracias a la documentación oficial de Microsoft https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/

Realizada para practicar para la renovación de la certificación **AZ-104** a **Marzo de 2024**

## CONCEPTOS BASICOS

### ¿Que es Azure App Service ?

<img align="center" src="/img/1ºimagenn.PNG"  />

Azure App Service es una solución de alojamiento de aplicaciones web totalmente gestionada que opera como una plataforma como servicio (PaaS) dentro de Azure. Esta plataforma permite a los desarrolladores centrarse en el desarrollo y diseño de sus aplicaciones, mientras que Azure se encarga de la infraestructura necesaria para ejecutar y escalar dichas aplicaciones de manera eficiente.

#### **Deployment Slots**

Utilizando el portal de Azure, se pueden añadir espacios de implementación a una aplicación web de App Service. Por ejemplo, puedes crear un espacio de implementación de **ensayo o staging** donde puedas cargar tu código para probarlo en Azure. Una vez que estés satisfecho con tu código, **puedes intercambiar** fácilmente el **espacio de implementación de ensayo** con el **espacio de implementación de producción**. Todo esto lo puedes realizar con unos pocos clics en el portal de Azure.



#### Soporte de integración/despliegue continuo

El portal de Azure ofrece integración y despliegue continuo listo para usar con Azure DevOps, GitHub, Bitbucket, FTP o un repositorio Git local en tu máquina de desarrollo. Conecta tu aplicación web con cualquiera de las fuentes mencionadas anteriormente, y App Service se encargará del resto automáticamente sincronizando tu código y cualquier cambio futuro en el código en la aplicación web. Además, con Azure DevOps, puedes definir tu propio proceso de compilación y liberación que compile tu código fuente, ejecute las pruebas, construya una liberación y finalmente despliegue la liberación en tu aplicación web cada vez que confirmes el código. Todo eso sucede implícitamente, sin necesidad de que intervengas.



#### Publicación integrada de Visual Studio y publicación FTP 

Además de poder configurar la integración/despliegue continuo para tu aplicación web, siempre puedes beneficiarte de la estrecha integración con Visual Studio para publicar tu aplicación web en Azure mediante la tecnología Web Deploy. App Service también admite la publicación basada en FTP para flujos de trabajo más tradicionales.

#### Soporte integrado de escalado automático (escalado automático o scale-out basado en carga real) 

El escalado hacia arriba/hacia abajo(mas recursos por ej, mas ram mas cores etc) o el escalado hacia fuera están integrados en la aplicación web. Dependiendo del uso de la aplicación web, puedes escalar tu aplicación hacia arriba/hacia abajo aumentando/disminuyendo los recursos de la máquina subyacente que está alojando tu aplicación web. Los recursos pueden ser el número de núcleos o la cantidad de RAM disponible.

Por otro lado, el escalado hacia fuera es la capacidad de aumentar el número de instancias de máquina que están ejecutando tu aplicación web.

### Creación Web App con C#

El portal ofrece un asistente para crear una aplicación web. Este asistente requiere los siguientes campos:

| Campo                                           | *D*escripción                                                |
| :---------------------------------------------- | :----------------------------------------------------------- |
| **Subscription**                                | Suscripción valida de azure                                  |
| **Resource group**                              | Grupo de recursos                                            |
| **App name**                                    | Nombre de la app, este nombre se convierte en parte de la URL de la app y ha de ser único entre todas las aplicaciones web de Azure App Service. |
| **Publish** o Publicación                       | Puedes implementar tu aplicación en App Service como código o como una imagen Docker lista para ejecutarse. Seleccionar la imagen Docker activará la pestaña Docker del asistente, donde proporcionarás información sobre el registro Docker desde el cual App Service recuperará tu imagen |
| **Runtime stack** o Pila de tiempo de ejecución | Si eliges implementar tu aplicación como código, App Service necesita saber qué tiempo de ejecución utiliza tu aplicación (ejemplos incluyen Node.js, Python, Java y .NET). Si implementas tu aplicación como una imagen Docker, no necesitarás elegir una pila de tiempo de ejecución, porque tu imagen la incluirá. |
| **Operating system**                            | App Service puede alojar aplicaciones en servidores Windows o Linux. |
| **Region**                                      | La región de Azure desde la cual se servirá tu aplicación    |
| **App Service Plan**                            | El plan el cual se usara                                     |

#### Sistemas operativos 

Al implementar tu aplicación como código, muchas de las pilas de tiempo de ejecución disponibles están limitadas a uno u otro sistema operativo. Después de elegir una pila de tiempo de ejecución, el interruptor indicará si tienes la opción de elegir el sistema operativo o no. Si tu pila de tiempo de ejecución objetivo está disponible en ambos sistemas operativos, selecciona aquel que uses para desarrollar y probar tu aplicación.

Si tu aplicación está empaquetada como una imagen Docker, elige el sistema operativo en el que tu imagen está diseñada para ejecutarse.

Seleccionar Windows activa la pestaña de Monitoreo, donde puedes habilitar Application Insights. Al habilitar esta característica, configuras tu aplicación para enviar automáticamente telemetría detallada de rendimiento al servicio de monitoreo de Application Insights sin necesidad de realizar cambios en tu código.

#### Planes de App Service 

Un plan de App Service es un conjunto de recursos de servidor virtual que ejecutan aplicaciones de App Service. El tamaño de un plan (a veces denominado su **SKU** o **nivel de precios**) **determina las características de rendimiento de los servidores virtuales** que ejecutan las aplicaciones asignadas al plan, así como las características de App Service a las que esas aplicaciones tienen acceso. Cada aplicación web de App Service que crees debe estar asignada a un solo plan de App Service que la ejecute.

Un solo plan de App Service puede alojar múltiples aplicaciones web de App Service. En la mayoría de los casos, el número de aplicaciones que puedes ejecutar en un solo plan está limitado por las características de rendimiento de las aplicaciones y las limitaciones de recursos del plan.

Un plan es la unidad de facturación de App Service. El tamaño de cada plan de App Service en tu suscripción, además de los recursos de ancho de banda que las aplicaciones desplegadas en esos planes usan, determina el precio que pagas. El número de aplicaciones web desplegadas en tus planes de App Service no tiene efecto en tu factura.

El asistente te ayudará a crear un nuevo plan si no dispones de ninguno.



#### Abrimos el portal de Azure con la misma cuenta con la que solicitamos el Sandbox desde Microsoft Learn:

<img align="center" src="/img/2ºimagenn.PNG"  />

Vamos a crear recurso y  buscamos Web en el buscador, seleccionamos Web App:

<img align="center" src="/img/3ºimagenn.PNG"  />

En la pestaña de datos básicos introduciremos los siguientes datos:

| Configuración                | Valor                                                  | Detalles                                                     |
| :--------------------------- | :----------------------------------------------------- | :----------------------------------------------------------- |
| **detalles del proyecto**    |                                                        |                                                              |
| Suscripción                  | Suscripción de conserjería                             | La aplicación web que está creando debe pertenecer a un grupo de recursos. Aquí, seleccione la suscripción de Azure a la que pertenece el grupo de recursos (o pertenecerá, si la está creando dentro del asistente). |
| Grupo de recursos            | Seleccionar learn-9fb05d04-fc2f-4112-8dfe-8843ff2ad804 | El grupo de recursos al que pertenecerá la aplicación web. Todos los recursos de Azure deben pertenecer a un grupo de recursos. |
| **Detalles de la instancia** |                                                        |                                                              |
| Nombre                       | *Introduzca un nombre único*                           | El nombre de su aplicación web. Este nombre formará parte de la dirección URL de la aplicación: *appname* .azurewebsites.net. El nombre que elija debe ser único entre todas las aplicaciones web de Azure. |
| Publicar                     | Código                                                 | El método que desea utilizar para publicar su aplicación. Al publicar una aplicación como código, también debe configurar **la pila de tiempo de ejecución** para preparar los recursos de App Service para ejecutar su aplicación. |
| Pila de tiempo de ejecución  | .NET 6 (LTS)                                           | La plataforma en la que se ejecutará su aplicación. Su elección puede afectar la posibilidad de elegir un sistema operativo: para algunas pilas de tiempo de ejecución, App Service solo admite un sistema operativo. |
| Sistema operativo            | linux                                                  | El sistema operativo utilizado en los servidores virtuales que ejecutarán su aplicación. |
| Región                       | Este de EE. UU.                                        | La región geográfica desde la que se alojará su aplicación.  |
| **Planes de precios**        |                                                        |                                                              |
| plan linux                   | Aceptar predeterminado                                 | El nombre del plan de App Service que impulsará su aplicación. De forma predeterminada, el asistente creará un nuevo plan en la misma región que la aplicación web. |
| plan de precios              | F1 gratis                                              | El nivel de precios del plan de servicio que se está creando. El plan de precios determina las características de rendimiento de los servidores virtuales que impulsan su aplicación y las funciones a las que tiene acceso. Seleccione **Libre F1** en el menú desplegable. |

<img align="center" src="/img/4ºimagenn.PNG"  />

Tras esto dejamos **cualquier otra config como predeterminada** y presionamos **revisar y crear**-> **crear**

Nuestra app y nuestro dominio queda así:

<img align="center" src="/img/5ºimagenn.PNG"  />

<img align="center" src="/img/6ºimagenn.PNG"  />

### Escribir código para implementar una aplicación web

El corazón de las herramientas CLI de .NET es la `dotnet` herramienta de línea de comandos. Con este comando, se creará un nuevo proyecto web ASP.NET Core.

Primero, instalemos la versión adecuada `dotnet`en Cloud Shell. Para este ejercicio, usaremos la versión 3.1.102 del SDK.

1. Ejecute los siguientes comandos en Azure Cloud Shell a la derecha para descargar e instalar dotnet:

```bash
wget -q -O - https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh | bash -s -- --version 6.0.404
export PATH="~/.dotnet:$PATH"
echo "export PATH=~/.dotnet:\$PATH" >> ~/.bashrc
```

Lo cual nos devuelve esto:

<img align="center" src="/img/7ºimagenn.PNG"  />

2. A continuación, ejecutrameos el siguiente comando para crear una nueva aplicación ASP.NET Core MVC denominada "BestBikeApp":

```bash
dotnet new mvc --name BestBikeApp
```

Que nos devuelve lo siguiente:

<img align="center" src="/img/8ºimagenn.PNG"  />

<img align="center" src="/img/9ºimagenn.PNG"  />

### Probemos la aplicación

Ejecutamos el siguiente comando para acceder a ella e iniciar la aplicación

```bash
cd BestBikeApp
dotnet run
```

<img align="center" src="/img/10ºimagenn.PNG"  />

Como podemos ver en la salida del comando se encuentra iniciado y escuchando en el puerto 5024, si abrimos otro terminal en otra pestaña y ejecutamos el siguiente comando deberemos ver el contenido de la pagina, vayamos a comprobarlo.

**Tras la realizar el ejercicio por segunda vez el puerto de escucha cambia así que hemos de estar pendiente cada vez que la iniciemos*

```bash
curl -kl http://127.0.0.1:5024/
```

<img align="center" src="/img/11ºimagenn.PNG"  />

Presionaremos ctrl+c para detener nuestra app en nuestra terminal principal



### Despliegue de código en App Service:

 **Implementación automatizada**

Azure admite la implementación automatizada directamente desde varias fuentes. Las siguientes opciones están disponibles:

- **Azure DevOps**: puede enviar su código a Azure DevOps, compilarlo en la nube, ejecutar las pruebas, generar una versión del código y, finalmente, enviar su código a una aplicación web de Azure.
- **GitHub**: Cuando conecta su repositorio de GitHub a Azure para una implementación automatizada, cualquier cambio que envíe a su rama de producción en GitHub se implementará automáticamente.
- **Bitbucket**: Debido a sus similitudes con GitHub, puedes configurar una implementación automatizada con Bitbucket.
- **OneDrive**: Debe tener una cuenta de Microsoft vinculada a una cuenta de OneDrive para implementar en Azure.
- **Dropbox**: Azure admite la implementación desde Dropbox.



Implementamos la aplicación .NET con implementación ZIP, insertamos el siguiente código en el bash

```bash
cd ~/BestBikeApp
dotnet publish -o pub
cd pub
zip -r site.zip *
```

Lo que devuelve algo así:

<img align="center" src="/img/12ºimagenn.PNG"  />

Finalmente, realizamos la implementación con `az webapp deployment source config-zip`. Reemplazamos  `<your-app-name>` que en mi caso seria `ramonapp ` en el siguiente comando con el nombre de nuestra aplicación web de Azure y lo ejecutamos

```bash
az webapp deployment source config-zip \
    --src site.zip \
    --resource-group learn-c34a26a4-e027-4309-9696-3052d548835a \
    --name <your-app-name>
```

Si nos devuelve un **código de estado 202** significará que la **implementación** fue **exitosa**:

<img align="center" src="/img/13ºimagenn.PNG"  />

Comprobamos volviendo al link de nuestra aplicación:

<img align="center" src="/img/14ºimagenn.PNG"  />
