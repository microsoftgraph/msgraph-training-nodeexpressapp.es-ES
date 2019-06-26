<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bd67e-101">Este tutorial le enseña a crear una aplicación Web de node. js Express que usa la API de Microsoft Graph para recuperar la información de calendario de un usuario.</span><span class="sxs-lookup"><span data-stu-id="bd67e-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="bd67e-102">Si prefiere descargar solo el tutorial completo, puede descargarlo de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="bd67e-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="bd67e-103">Descargue el [Inicio rápido de node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obtener el código de trabajo en minutos.</span><span class="sxs-lookup"><span data-stu-id="bd67e-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="bd67e-104">Descargue o Clone el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="bd67e-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd67e-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bd67e-105">Prerequisites</span></span>

<span data-ttu-id="bd67e-106">Antes de comenzar esta demostración, debe tener [node. js](https://nodejs.org) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="bd67e-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="bd67e-107">Si no tiene node. js, visite el vínculo anterior para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="bd67e-107">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="bd67e-108">Este tutorial se ha escrito con la versión de nodo 10.15.3.</span><span class="sxs-lookup"><span data-stu-id="bd67e-108">This tutorial was written with Node version 10.15.3.</span></span> <span data-ttu-id="bd67e-109">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="bd67e-109">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="bd67e-110">Vea el tutorial</span><span class="sxs-lookup"><span data-stu-id="bd67e-110">Watch the tutorial</span></span>

<span data-ttu-id="bd67e-111">Este módulo se ha registrado y está disponible en el canal de YouTube de desarrollo de Office.</span><span class="sxs-lookup"><span data-stu-id="bd67e-111">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/n6q8Cm-pTYY]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="bd67e-112">Comentarios</span><span class="sxs-lookup"><span data-stu-id="bd67e-112">Feedback</span></span>

<span data-ttu-id="bd67e-113">Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="bd67e-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>
