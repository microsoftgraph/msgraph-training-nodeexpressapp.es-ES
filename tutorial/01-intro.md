<!-- markdownlint-disable MD002 MD041 -->

Este tutorial le enseña a crear una aplicación web de Node.js Express que use la API de Microsoft Graph para recuperar información de calendario para un usuario.

> [!TIP]
> Si prefiere descargar el tutorial completado, puede descargarlo de dos maneras.
>
> - Descargue el [Node.js rápido para](https://developer.microsoft.com/graph/quick-start?platform=option-node) obtener el código de trabajo en minutos.
> - Descargue o clone el [repositorio GitHub archivo](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar esta demostración, debe haber [Node.js](https://nodejs.org) instalado en el equipo de desarrollo. Si no tiene Node.js, visite el vínculo anterior para ver las opciones de descarga.

> [!NOTE]
> Windows usuarios deben instalar Python y Visual Studio Build Tools para admitir módulos NPM que deben compilarse desde C/C++. El Node.js en Windows ofrece una opción para instalar automáticamente estas herramientas. Como alternativa, puede seguir las instrucciones en [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .

También debe tener una cuenta personal de Microsoft con un buzón en Outlook.com, o una cuenta de Trabajo o escuela de Microsoft. Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede registrarse [para obtener una nueva cuenta personal de Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puedes [registrarte en el programa Office 365 desarrolladores](https://developer.microsoft.com/office/dev-program) para obtener una suscripción Office 365 gratuita.

> [!NOTE]
> Este tutorial se escribió con Node versión 14.15.0. Los pasos de esta guía pueden funcionar con otras versiones, pero eso no se ha probado.

## <a name="feedback"></a>Comentarios

Proporcione cualquier comentario sobre este tutorial en el [repositorio GitHub archivo](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).
