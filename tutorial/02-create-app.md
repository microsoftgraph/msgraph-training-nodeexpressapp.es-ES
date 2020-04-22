<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5cd03-101">En este ejercicio, usará [Express](http://expressjs.com/) para crear una aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="5cd03-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="5cd03-102">Abra la CLI, vaya a un directorio donde tenga derechos para crear archivos y ejecute el siguiente comando para crear una nueva aplicación Express que use [manillares](http://handlebarsjs.com/) como motor de representación.</span><span class="sxs-lookup"><span data-stu-id="5cd03-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="5cd03-103">El generador de Express crea un nuevo directorio `graph-tutorial` al que se llama y scaffolding una aplicación Express.</span><span class="sxs-lookup"><span data-stu-id="5cd03-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="5cd03-104">Desplácese hasta `graph-tutorial` el directorio y escriba el siguiente comando para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="5cd03-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="5cd03-105">Ejecute el siguiente comando para actualizar los paquetes de nodos con vulnerabilidades de las que se ha informado.</span><span class="sxs-lookup"><span data-stu-id="5cd03-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="5cd03-106">Use el siguiente comando para iniciar un servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="5cd03-106">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="5cd03-107">Abra el explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="5cd03-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="5cd03-108">Si todo funciona, verá un mensaje "Bienvenido a exprés".</span><span class="sxs-lookup"><span data-stu-id="5cd03-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="5cd03-109">Si no ve ese mensaje, consulte la [Guía de introducción rápida](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="5cd03-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="5cd03-110">Instalación de paquetes de nodos</span><span class="sxs-lookup"><span data-stu-id="5cd03-110">Install Node packages</span></span>

<span data-ttu-id="5cd03-111">Antes de continuar, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="5cd03-111">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="5cd03-112">[dotenv](https://github.com/motdotla/dotenv) para cargar los valores de un archivo. env.</span><span class="sxs-lookup"><span data-stu-id="5cd03-112">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="5cd03-113">[momento](https://github.com/moment/moment/) para dar formato a valores de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="5cd03-113">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="5cd03-114">mensajes de error de [Connect-Flash](https://github.com/jaredhanson/connect-flash) a Flash en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5cd03-114">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="5cd03-115">[Express-Session](https://github.com/expressjs/session) para almacenar valores en una sesión del lado servidor de la memoria.</span><span class="sxs-lookup"><span data-stu-id="5cd03-115">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="5cd03-116">[Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) para autenticar y obtener tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="5cd03-116">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="5cd03-117">[simple: OAuth2 para la](https://github.com/lelylan/simple-oauth2) administración de tokens.</span><span class="sxs-lookup"><span data-stu-id="5cd03-117">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="5cd03-118">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5cd03-118">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="5cd03-119">[isomórfico: fetch](https://github.com/matthew-andrews/isomorphic-fetch) para subllenar la búsqueda para el nodo.</span><span class="sxs-lookup"><span data-stu-id="5cd03-119">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="5cd03-120">Se requiere un polyfill de Fetch para la `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="5cd03-120">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="5cd03-121">Vea la [biblioteca cliente de JavaScript de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="5cd03-121">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="5cd03-122">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="5cd03-122">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
    ```

    > [!TIP]
    > <span data-ttu-id="5cd03-123">Es posible que los usuarios de Windows reciban el siguiente mensaje de error al intentar instalar estos paquetes en Windows.</span><span class="sxs-lookup"><span data-stu-id="5cd03-123">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="5cd03-124">Para solucionar el error, ejecute el siguiente comando para instalar las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (Administrador) que instala las herramientas de compilación de VS y Python.</span><span class="sxs-lookup"><span data-stu-id="5cd03-124">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="5cd03-125">Actualice la aplicación para usar el `connect-flash` software `express-session` intermedio y.</span><span class="sxs-lookup"><span data-stu-id="5cd03-125">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="5cd03-126">Abra el `./app.js` archivo y agregue la siguiente `require` instrucción en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="5cd03-126">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. <span data-ttu-id="5cd03-127">Agregue el siguiente código inmediatamente después de `var app = express();` la línea.</span><span class="sxs-lookup"><span data-stu-id="5cd03-127">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="5cd03-128">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="5cd03-128">Design the app</span></span>

<span data-ttu-id="5cd03-129">En esta sección, implementará la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5cd03-129">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="5cd03-130">Abra el `./views/layout.hbs` archivo y reemplace todo el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5cd03-130">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="5cd03-131">Este código agrega un [bootstrap](http://getbootstrap.com/) para los estilos sencillos y la [fuente maravilla](https://fontawesome.com/) para algunos iconos simples.</span><span class="sxs-lookup"><span data-stu-id="5cd03-131">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="5cd03-132">También define un diseño global con una barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="5cd03-132">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="5cd03-133">Abra `./public/stylesheets/style.css` y reemplace todo el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="5cd03-133">Open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="5cd03-134">Abra el `./views/index.hbs` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="5cd03-134">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="5cd03-135">Abra el `./routes/index.js` archivo y reemplace el código existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="5cd03-135">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="5cd03-136">Guarde todos los cambios y reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="5cd03-136">Save all of your changes and restart the server.</span></span> <span data-ttu-id="5cd03-137">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="5cd03-137">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la Página principal rediseñada](./images/create-app-01.png)
