<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c8d8a-101">En este ejercicio, usará [Express para](http://expressjs.com/) crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="c8d8a-102">Abre la CLI, navega a un directorio donde tienes derechos para crear archivos y ejecuta el siguiente comando para crear una nueva aplicación express que use [Handlebars](http://handlebarsjs.com/) como motor de representación.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="c8d8a-103">El generador express crea un nuevo directorio llamado `graph-tutorial` y aplica scaffolding a una aplicación Express.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="c8d8a-104">Vaya al directorio `graph-tutorial` y escriba el siguiente comando para instalar dependencias.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="c8d8a-105">Ejecuta el siguiente comando para actualizar paquetes de Node con vulnerabilidades notificadas.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="c8d8a-106">Ejecute el siguiente comando para actualizar la versión de Express y otras dependencias.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="c8d8a-107">Use el siguiente comando para iniciar un servidor web local.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="c8d8a-108">Abra el explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="c8d8a-109">Si todo funciona, verá el mensaje "Bienvenido a Express".</span><span class="sxs-lookup"><span data-stu-id="c8d8a-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="c8d8a-110">Si no ve ese mensaje, consulte la guía de introducción [de Express.](http://expressjs.com/starter/generator.html)</span><span class="sxs-lookup"><span data-stu-id="c8d8a-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="c8d8a-111">Instalar paquetes de Node</span><span class="sxs-lookup"><span data-stu-id="c8d8a-111">Install Node packages</span></span>

<span data-ttu-id="c8d8a-112">Antes de seguir adelante, instala algunos paquetes adicionales que usarás más adelante:</span><span class="sxs-lookup"><span data-stu-id="c8d8a-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="c8d8a-113">[dotenv](https://github.com/motdotla/dotenv) para cargar valores desde un archivo .env.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="c8d8a-114">[momento](https://github.com/moment/moment/) para dar formato a los valores de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="c8d8a-115">[windows-iana](https://github.com/rubenillodo/windows-iana) para traducir nombres de zona horaria de Windows a los IDs de zona horaria de IANA.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="c8d8a-116">[conectar-flash](https://github.com/jaredhanson/connect-flash) a los mensajes de error de flash en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="c8d8a-117">[express-session](https://github.com/expressjs/session) para almacenar valores en una sesión del lado servidor en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="c8d8a-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) para permitir que los controladores de ruta devuelvan una promesa.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="c8d8a-119">[express-validator](https://github.com/express-validator/express-validator) para analizar y validar datos de formulario.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="c8d8a-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar y obtener tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="c8d8a-121">[uuid](https://github.com/uuidjs/uuid) usado por msal-node para generar GUID.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="c8d8a-122">[microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="c8d8a-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="c8d8a-124">Se requiere un polyfill de captura para la `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="c8d8a-125">Vea la wiki de la biblioteca [cliente de JavaScript de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="c8d8a-126">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="c8d8a-127">Los usuarios de Windows pueden recibir el siguiente mensaje de error al intentar instalar estos paquetes en Windows.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="c8d8a-128">Para resolver el error, ejecute el siguiente comando para instalar Las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (administrador) que instala las herramientas de compilación vs y Python.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="c8d8a-129">Actualice la aplicación para usar el `connect-flash` software intermedio `express-session` y.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="c8d8a-130">Abra **./app.js** y agregue la siguiente `require` instrucción en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="c8d8a-131">Agregue el siguiente código inmediatamente después de la `var app = express();` línea.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="c8d8a-132">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c8d8a-132">Design the app</span></span>

<span data-ttu-id="c8d8a-133">En esta sección, implementará la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="c8d8a-134">Abra **./views/layout.vhs** y reemplace todo el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="c8d8a-135">Este código agrega [Bootstrap para](http://getbootstrap.com/) estilos sencillos y [Font Awesome](https://fontawesome.com/) para algunos iconos simples.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="c8d8a-136">También define un diseño global con una barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="c8d8a-137">Abra **./public/stylesheets/style.css** y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="c8d8a-138">Abra **./views/index.vhs** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="c8d8a-139">Abra **./routes/index.js** y reemplace el código existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="c8d8a-140">Guarde todos los cambios y reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="c8d8a-141">Ahora, la aplicación debería tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="c8d8a-141">Now, the app should look very different.</span></span>

    ![Captura de pantalla de la página principal rediseñada](./images/create-app-01.png)
