<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2be27-101">En este ejercicio, [usarás Express](http://expressjs.com/) para crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2be27-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="2be27-102">Abra la CLI, vaya a un directorio donde tenga derechos para crear archivos y ejecute el siguiente comando para crear una nueva aplicación Express que use [Handlebars](http://handlebarsjs.com/) como motor de representación.</span><span class="sxs-lookup"><span data-stu-id="2be27-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="2be27-103">El generador de Express crea un nuevo directorio llamado `graph-tutorial` y scaffolding de una aplicación Express.</span><span class="sxs-lookup"><span data-stu-id="2be27-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="2be27-104">Vaya al directorio `graph-tutorial` y escriba el siguiente comando para instalar dependencias.</span><span class="sxs-lookup"><span data-stu-id="2be27-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="2be27-105">Ejecute el siguiente comando para actualizar los paquetes de Node con vulnerabilidades notificadas.</span><span class="sxs-lookup"><span data-stu-id="2be27-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="2be27-106">Ejecute el siguiente comando para actualizar la versión de Express y otras dependencias.</span><span class="sxs-lookup"><span data-stu-id="2be27-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. <span data-ttu-id="2be27-107">Use el siguiente comando para iniciar un servidor web local.</span><span class="sxs-lookup"><span data-stu-id="2be27-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="2be27-108">Abra el explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="2be27-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="2be27-109">Si todo funciona, verá un mensaje "Bienvenido a Express".</span><span class="sxs-lookup"><span data-stu-id="2be27-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="2be27-110">Si no ve ese mensaje, consulte la guía [de introducción de Express](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="2be27-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="2be27-111">Instalar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="2be27-111">Install Node packages</span></span>

<span data-ttu-id="2be27-112">Antes de seguir adelante, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="2be27-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="2be27-113">[dotenv para](https://github.com/motdotla/dotenv) cargar valores desde un archivo .env.</span><span class="sxs-lookup"><span data-stu-id="2be27-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="2be27-114">[date-fns](https://github.com/date-fns/date-fns) para dar formato a los valores de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="2be27-114">[date-fns](https://github.com/date-fns/date-fns) for formatting date/time values.</span></span>
- <span data-ttu-id="2be27-115">[windows-iana para](https://github.com/rubenillodo/windows-iana) traducir Windows nombres de zona horaria a los IDs de zona horaria de IANA.</span><span class="sxs-lookup"><span data-stu-id="2be27-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="2be27-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span><span class="sxs-lookup"><span data-stu-id="2be27-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="2be27-117">[express-session para](https://github.com/expressjs/session) almacenar valores en una sesión del lado servidor en memoria.</span><span class="sxs-lookup"><span data-stu-id="2be27-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="2be27-118">[express-promise-router para](https://github.com/express-promise-router/express-promise-router) permitir que los controladores de ruta devuelvan una promesa.</span><span class="sxs-lookup"><span data-stu-id="2be27-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="2be27-119">[express-validator](https://github.com/express-validator/express-validator) para analizar y validar datos de formulario.</span><span class="sxs-lookup"><span data-stu-id="2be27-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="2be27-120">[msal-node para](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) autenticar y obtener tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="2be27-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="2be27-121">[microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2be27-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="2be27-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span><span class="sxs-lookup"><span data-stu-id="2be27-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="2be27-123">Se requiere un polyfill de captura para la `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="2be27-123">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="2be27-124">Consulta el [wiki de biblioteca Graph cliente de JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) de Microsoft para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="2be27-124">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="2be27-125">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="2be27-125">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > <span data-ttu-id="2be27-126">Windows usuarios pueden obtener el siguiente mensaje de error al intentar instalar estos paquetes en Windows.</span><span class="sxs-lookup"><span data-stu-id="2be27-126">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="2be27-127">Para resolver el error, ejecute el siguiente comando para instalar las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (administrador), que instala vs Build Tools y Python.</span><span class="sxs-lookup"><span data-stu-id="2be27-127">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="2be27-128">Actualice la aplicación para usar el `connect-flash` software intermedio `express-session` y.</span><span class="sxs-lookup"><span data-stu-id="2be27-128">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="2be27-129">Abra **./app.js** y agregue la siguiente `require` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="2be27-129">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="2be27-130">Agregue el siguiente código inmediatamente después de la `var app = express();` línea.</span><span class="sxs-lookup"><span data-stu-id="2be27-130">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="2be27-131">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="2be27-131">Design the app</span></span>

<span data-ttu-id="2be27-132">En esta sección, implementará la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2be27-132">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="2be27-133">Abra **./views/layout.hbs** y reemplace todo el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="2be27-133">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="2be27-134">Este código agrega [Bootstrap](http://getbootstrap.com/) para un estilo sencillo y [Bootstrap](https://fontawesome.com/) para algunos iconos simples.</span><span class="sxs-lookup"><span data-stu-id="2be27-134">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="2be27-135">También define un diseño global con una barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="2be27-135">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="2be27-136">Abra **./public/stylesheets/style.css** y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2be27-136">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="2be27-137">Abra **./views/index.hbs** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2be27-137">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="2be27-138">Abra **./routes/index.js** y reemplace el código existente por el siguiente.</span><span class="sxs-lookup"><span data-stu-id="2be27-138">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="2be27-139">Guarde todos los cambios y reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="2be27-139">Save all of your changes and restart the server.</span></span> <span data-ttu-id="2be27-140">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="2be27-140">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la página de inicio rediseñada](./images/create-app-01.png)
