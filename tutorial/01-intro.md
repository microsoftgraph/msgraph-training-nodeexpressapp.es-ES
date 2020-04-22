<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1dc51-101">Este tutorial le enseña a crear una aplicación Web de node. js Express que usa la API de Microsoft Graph para recuperar la información de calendario de un usuario.</span><span class="sxs-lookup"><span data-stu-id="1dc51-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="1dc51-102">Si prefiere descargar solo el tutorial completo, puede descargarlo de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="1dc51-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="1dc51-103">Descargue el [Inicio rápido de node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obtener el código de trabajo en minutos.</span><span class="sxs-lookup"><span data-stu-id="1dc51-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="1dc51-104">Descargue o Clone el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="1dc51-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dc51-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1dc51-105">Prerequisites</span></span>

<span data-ttu-id="1dc51-106">Antes de comenzar esta demostración, debe tener [node. js](https://nodejs.org) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1dc51-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="1dc51-107">Si no tiene node. js, visite el vínculo anterior para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="1dc51-107">If you do not have Node.js, visit the previous link for download options.</span></span>

<span data-ttu-id="1dc51-108">También debe tener una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1dc51-108">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="1dc51-109">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="1dc51-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="1dc51-110">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="1dc51-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="1dc51-111">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="1dc51-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="1dc51-112">Este tutorial se ha escrito con la versión de nodo 12.6.1.</span><span class="sxs-lookup"><span data-stu-id="1dc51-112">This tutorial was written with Node version 12.6.1.</span></span> <span data-ttu-id="1dc51-113">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="1dc51-113">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="1dc51-114">Vea el tutorial</span><span class="sxs-lookup"><span data-stu-id="1dc51-114">Watch the tutorial</span></span>

<span data-ttu-id="1dc51-115">Este módulo se ha registrado y está disponible en el canal de YouTube de desarrollo de Office.</span><span class="sxs-lookup"><span data-stu-id="1dc51-115">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/n6q8Cm-pTYY]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="1dc51-116">Comentarios</span><span class="sxs-lookup"><span data-stu-id="1dc51-116">Feedback</span></span>

<span data-ttu-id="1dc51-117">Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="1dc51-117">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>
