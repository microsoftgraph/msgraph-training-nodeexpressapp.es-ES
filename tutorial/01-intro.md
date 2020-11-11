<!-- markdownlint-disable MD002 MD041 -->

Este tutorial le enseña a crear una aplicación Web de Node.js Express que use la API de Microsoft Graph para recuperar la información de calendario de un usuario.

> [!TIP]
> Si prefiere descargar solo el tutorial completo, puede descargarlo de dos maneras.
>
> - Descargue el [Node.js Quick Start](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obtener el código de trabajo en minutos.
> - Descargue o Clone el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar esta demostración, debe tener [Node.js](https://nodejs.org) instalado en el equipo de desarrollo. Si no tiene Node.js, visite el vínculo anterior para las opciones de descarga.

> [!NOTE]
> Es posible que los usuarios de Windows necesiten instalar las herramientas de compilación de Python y Visual Studio para admitir los módulos NPM que deben compilarse desde C/C++. El programa de instalación de Node.js en Windows ofrece una opción para instalar automáticamente estas herramientas. Como alternativa, puede seguir las instrucciones que se indican en [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .

También debe tener una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft. Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

> [!NOTE]
> Este tutorial se ha escrito con la versión de nodo 12.18.4. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.

## <a name="feedback"></a>Comentarios

Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).
