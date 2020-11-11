<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="297d2-101">En este ejercicio, usará [Express](http://expressjs.com/) para crear una aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="297d2-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="297d2-102">Abra la CLI, vaya a un directorio donde tenga derechos para crear archivos y ejecute el siguiente comando para crear una nueva aplicación Express que use [manillares](http://handlebarsjs.com/) como motor de representación.</span><span class="sxs-lookup"><span data-stu-id="297d2-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="297d2-103">El generador de Express crea un nuevo directorio al que se llama `graph-tutorial` y scaffolding una aplicación Express.</span><span class="sxs-lookup"><span data-stu-id="297d2-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="297d2-104">Desplácese hasta el `graph-tutorial` Directorio y escriba el siguiente comando para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="297d2-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="297d2-105">Ejecute el siguiente comando para actualizar los paquetes de nodos con vulnerabilidades de las que se ha informado.</span><span class="sxs-lookup"><span data-stu-id="297d2-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="297d2-106">Ejecute el siguiente comando para actualizar la versión de Express y otras dependencias.</span><span class="sxs-lookup"><span data-stu-id="297d2-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="297d2-107">Use el siguiente comando para iniciar un servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="297d2-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="297d2-108">Abra el explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="297d2-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="297d2-109">Si todo funciona, verá un mensaje "Bienvenido a exprés".</span><span class="sxs-lookup"><span data-stu-id="297d2-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="297d2-110">Si no ve ese mensaje, consulte la [Guía de introducción rápida](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="297d2-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="297d2-111">Instalación de paquetes de nodos</span><span class="sxs-lookup"><span data-stu-id="297d2-111">Install Node packages</span></span>

<span data-ttu-id="297d2-112">Antes de continuar, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="297d2-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="297d2-113">[dotenv](https://github.com/motdotla/dotenv) para cargar los valores de un archivo. env.</span><span class="sxs-lookup"><span data-stu-id="297d2-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="297d2-114">[momento](https://github.com/moment/moment/) para dar formato a valores de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="297d2-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="297d2-115">[Windows-IANA](https://github.com/rubenillodo/windows-iana) para traducir los nombres de zona horaria de Windows a identificadores de zona horaria de IANA.</span><span class="sxs-lookup"><span data-stu-id="297d2-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="297d2-116">mensajes de error de [Connect-Flash](https://github.com/jaredhanson/connect-flash) a Flash en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="297d2-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="297d2-117">[Express-Session](https://github.com/expressjs/session) para almacenar valores en una sesión del lado servidor de la memoria.</span><span class="sxs-lookup"><span data-stu-id="297d2-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="297d2-118">[Express-Promise-enrutador](https://github.com/express-promise-router/express-promise-router) para permitir que los controladores de ruta devuelvan una promesa.</span><span class="sxs-lookup"><span data-stu-id="297d2-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="297d2-119">[validador expreso](https://github.com/express-validator/express-validator) para analizar y validar datos de formulario.</span><span class="sxs-lookup"><span data-stu-id="297d2-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="297d2-120">[msal-nodo](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar y obtener tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="297d2-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="297d2-121">[UUID](https://github.com/uuidjs/uuid) usado por msal-node para generar GUID.</span><span class="sxs-lookup"><span data-stu-id="297d2-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="297d2-122">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="297d2-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="297d2-123">[isomórfico: fetch](https://github.com/matthew-andrews/isomorphic-fetch) para subllenar la búsqueda para el nodo.</span><span class="sxs-lookup"><span data-stu-id="297d2-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="297d2-124">Se requiere un polyfill de Fetch para la `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="297d2-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="297d2-125">Vea la [biblioteca cliente de JavaScript de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="297d2-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="297d2-126">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="297d2-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="297d2-127">Es posible que los usuarios de Windows reciban el siguiente mensaje de error al intentar instalar estos paquetes en Windows.</span><span class="sxs-lookup"><span data-stu-id="297d2-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="297d2-128">Para solucionar el error, ejecute el siguiente comando para instalar las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (Administrador) que instala las herramientas de compilación de VS y Python.</span><span class="sxs-lookup"><span data-stu-id="297d2-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="297d2-129">Actualice la aplicación para usar el `connect-flash` `express-session` software intermedio y.</span><span class="sxs-lookup"><span data-stu-id="297d2-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="297d2-130">Abra **./app.js** y agregue la siguiente `require` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="297d2-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="297d2-131">Agregue el siguiente código inmediatamente después de la `var app = express();` línea.</span><span class="sxs-lookup"><span data-stu-id="297d2-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="297d2-132">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="297d2-132">Design the app</span></span>

<span data-ttu-id="297d2-133">En esta sección, implementará la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="297d2-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="297d2-134">Abra **./views/layout.HBS** y reemplace todo el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="297d2-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="297d2-135">Este código agrega un [bootstrap](http://getbootstrap.com/) para los estilos sencillos y la [fuente maravilla](https://fontawesome.com/) para algunos iconos simples.</span><span class="sxs-lookup"><span data-stu-id="297d2-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="297d2-136">También define un diseño global con una barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="297d2-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="297d2-137">Abra **./Public/Stylesheets/Style.CSS** y reemplace todo el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="297d2-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="297d2-138">Abra **./views/index.HBS** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="297d2-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="297d2-139">Abra el **index.js./Routes/** y reemplace el código existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="297d2-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="297d2-140">Guarde todos los cambios y reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="297d2-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="297d2-141">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="297d2-141">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la Página principal rediseñada](./images/create-app-01.png)
