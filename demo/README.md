# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- [Node.js](https://nodejs.org) instalado en el equipo de desarrollo. Si no tiene Node.js, visite el vínculo anterior para ver las opciones de descarga. (**Nota: este** tutorial se escribió con Node versión 14.15.0. Los pasos de esta guía pueden funcionar con otras versiones, pero eso no se ha probado).
- Una cuenta personal de Microsoft con un buzón en Outlook.com, o una cuenta de Trabajo o escuela de Microsoft.

Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede registrarse [para obtener una nueva cuenta personal de Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puedes [registrarte en el programa Office 365 desarrolladores](https://developer.microsoft.com/office/dev-program) para obtener una suscripción Office 365 gratuita.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar una aplicación web con el Centro Azure Active Directory administración

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Captura de pantalla de los registros de la aplicación ](/tutorial/images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `Node.js Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección**, establezca la primera lista desplegable en `Web` y establezca el valor `http://localhost:3000/auth/callback`.

    ![Captura de pantalla de la página Registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. Elija **Registrar**. En la **Node.js Graph tutorial,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el siguiente paso.

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción** y seleccione una de las opciones de **Expira** y seleccione **Agregar**.

    ![Una captura de pantalla del cuadro de diálogo Agregar un cliente secreto](/tutorial/images/aad-new-client-secret.png)

1. Copie el valor del secreto del cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

    ![Una captura de pantalla del nuevo secreto de cliente agregado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Cambie el nombre `example.env` del archivo a `.env` .
1. Edite `.env` el archivo y realice los siguientes cambios.
    1. Reemplace `YOUR_CLIENT_SECRET_HERE` por el **id. de** aplicación que obtuvo del Portal de registro de aplicaciones.
    1. Reemplace `YOUR_APP_PASSWORD_HERE` por la contraseña que obtuvo del Portal de registro de aplicaciones.
1. En la interfaz de línea de comandos (CLI), vaya a este directorio y ejecute el siguiente comando para instalar los requisitos.

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Ejecute el siguiente comando en la CLI para iniciar la aplicación.

    ```Shell
    npm start
    ```

1. Abra un explorador y vaya a `http://localhost:3000`.
