# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="ee593-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ee593-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee593-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ee593-102">Prerequisites</span></span>

<span data-ttu-id="ee593-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee593-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="ee593-104">[Node.js](https://nodejs.org) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ee593-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="ee593-105">Si no tiene Node.js, visite el vínculo anterior para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="ee593-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="ee593-106">( **Nota:** este tutorial se ha escrito con la versión de nodo 12.6.1.</span><span class="sxs-lookup"><span data-stu-id="ee593-106">( **Note:** This tutorial was written with Node version 12.6.1.</span></span> <span data-ttu-id="ee593-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="ee593-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="ee593-108">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ee593-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="ee593-109">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="ee593-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="ee593-110">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="ee593-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="ee593-111">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="ee593-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="ee593-112">Registro de una aplicación web con el centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ee593-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="ee593-113">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee593-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="ee593-114">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="ee593-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="ee593-115">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="ee593-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="ee593-116">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ee593-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="ee593-117">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="ee593-117">Select **New registration**.</span></span> <span data-ttu-id="ee593-118">En la página **Registrar una aplicación** , establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="ee593-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ee593-119">Establezca **Nombre** como `Node.js Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="ee593-119">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="ee593-120">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="ee593-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ee593-121">En **URI de redirección** , establezca la primera lista desplegable en `Web` y establezca el valor `http://localhost:3000/auth/callback`.</span><span class="sxs-lookup"><span data-stu-id="ee593-121">Under **Redirect URI** , set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="ee593-123">Elija **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="ee593-123">Choose **Register**.</span></span> <span data-ttu-id="ee593-124">En la página del **tutorial deNode.js Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ee593-124">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="ee593-126">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="ee593-126">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="ee593-127">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="ee593-127">Select the **New client secret** button.</span></span> <span data-ttu-id="ee593-128">Escriba un valor en **Descripción** y seleccione una de las opciones de **Expira** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ee593-128">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Captura de pantalla del cuadro de diálogo Agregar un secreto de cliente](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="ee593-130">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="ee593-130">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ee593-131">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="ee593-131">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ee593-132">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="ee593-132">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Captura de pantalla del secreto de cliente recién agregado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="ee593-134">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="ee593-134">Configure the sample</span></span>

1. <span data-ttu-id="ee593-135">Cambie el nombre del `example.env` archivo a `.env` .</span><span class="sxs-lookup"><span data-stu-id="ee593-135">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="ee593-136">Edite el `.env` archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="ee593-136">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="ee593-137">Reemplace `YOUR_CLIENT_SECRET_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ee593-137">Replace `YOUR_CLIENT_SECRET_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="ee593-138">Reemplace `YOUR_APP_PASSWORD_HERE` por la contraseña que obtuvo en el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ee593-138">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="ee593-139">En la interfaz de línea de comandos (CLI), navegue a este directorio y ejecute el siguiente comando para instalar los requisitos.</span><span class="sxs-lookup"><span data-stu-id="ee593-139">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="ee593-140">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="ee593-140">Run the sample</span></span>

1. <span data-ttu-id="ee593-141">Ejecute el siguiente comando en su CLI para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee593-141">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="ee593-142">Abra un explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="ee593-142">Open a browser and browse to `http://localhost:3000`.</span></span>
