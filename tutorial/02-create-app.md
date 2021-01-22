<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, usará [Express para](http://expressjs.com/) crear una aplicación web.

1. Abre la CLI, navega a un directorio donde tienes derechos para crear archivos y ejecuta el siguiente comando para crear una nueva aplicación express que use [Handlebars](http://handlebarsjs.com/) como motor de representación.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    El generador express crea un nuevo directorio llamado `graph-tutorial` y aplica scaffolding a una aplicación Express.

1. Vaya al directorio `graph-tutorial` y escriba el siguiente comando para instalar dependencias.

    ```Shell
    npm install
    ```

1. Ejecuta el siguiente comando para actualizar paquetes de Node con vulnerabilidades notificadas.

    ```Shell
    npm audit fix
    ```

1. Ejecute el siguiente comando para actualizar la versión de Express y otras dependencias.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. Use el siguiente comando para iniciar un servidor web local.

    ```Shell
    npm start
    ```

1. Abra el explorador y vaya a `http://localhost:3000`. Si todo funciona, verá el mensaje "Bienvenido a Express". Si no ve ese mensaje, consulte la guía de introducción [de Express.](http://expressjs.com/starter/generator.html)

## <a name="install-node-packages"></a>Instalar paquetes de Node

Antes de seguir adelante, instala algunos paquetes adicionales que usarás más adelante:

- [dotenv](https://github.com/motdotla/dotenv) para cargar valores desde un archivo .env.
- [momento](https://github.com/moment/moment/) para dar formato a los valores de fecha y hora.
- [windows-iana](https://github.com/rubenillodo/windows-iana) para traducir nombres de zona horaria de Windows a los IDs de zona horaria de IANA.
- [conectar-flash](https://github.com/jaredhanson/connect-flash) a los mensajes de error de flash en la aplicación.
- [express-session](https://github.com/expressjs/session) para almacenar valores en una sesión del lado servidor en memoria.
- [express-promise-router](https://github.com/express-promise-router/express-promise-router) para permitir que los controladores de ruta devuelvan una promesa.
- [express-validator](https://github.com/express-validator/express-validator) para analizar y validar datos de formulario.
- [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar y obtener tokens de acceso.
- [uuid](https://github.com/uuidjs/uuid) usado por msal-node para generar GUID.
- [microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node. Se requiere un polyfill de captura para la `microsoft-graph-client` biblioteca. Vea la wiki de la biblioteca [cliente de JavaScript de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obtener más información.

1. Ejecute el siguiente comando en la CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > Los usuarios de Windows pueden recibir el siguiente mensaje de error al intentar instalar estos paquetes en Windows.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Para resolver el error, ejecute el siguiente comando para instalar Las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (administrador) que instala las herramientas de compilación vs y Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Actualice la aplicación para usar el `connect-flash` software intermedio `express-session` y. Abra **./app.js** y agregue la siguiente `require` instrucción en la parte superior del archivo.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Agregue el siguiente código inmediatamente después de la `var app = express();` línea.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, implementará la interfaz de usuario de la aplicación.

1. Abra **./views/layout.vhs** y reemplace todo el contenido por el código siguiente.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    Este código agrega [Bootstrap para](http://getbootstrap.com/) estilos sencillos y [Font Awesome](https://fontawesome.com/) para algunos iconos simples. También define un diseño global con una barra de navegación.

1. Abra **./public/stylesheets/style.css** y reemplace todo su contenido por lo siguiente.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Abra **./views/index.vhs** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Abra **./routes/index.js** y reemplace el código existente por lo siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Guarde todos los cambios y reinicie el servidor. Ahora, la aplicación debería tener un aspecto muy diferente.

    ![Captura de pantalla de la página principal rediseñada](./images/create-app-01.png)
