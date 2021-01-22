<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="27293-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="27293-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="27293-102">Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="27293-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="27293-103">En este paso, integrará la [biblioteca msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="27293-103">In this step you will integrate the [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) library into the application.</span></span>

1. <span data-ttu-id="27293-104">Cree un archivo denominado **.env en** la raíz de la aplicación y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="27293-104">Create a new file named **.env** in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="27293-105">Reemplace con el identificador de aplicación del Portal de registro de aplicaciones y reemplace `YOUR_CLIENT_SECRET_HERE` por el secreto de cliente que `YOUR_APP_SECRET_HERE` generó.</span><span class="sxs-lookup"><span data-stu-id="27293-105">Replace `YOUR_CLIENT_SECRET_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the client secret you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="27293-106">Si usas el control de código fuente como Git, ahora sería un buen momento para excluir el archivo **.env** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="27293-106">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="27293-107">Abra **./app.js** y agregue la siguiente línea en la parte superior del archivo para cargar el **archivo .env.**</span><span class="sxs-lookup"><span data-stu-id="27293-107">Open **./app.js** and add the following line to the top of the file to load the **.env** file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="27293-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="27293-108">Implement sign-in</span></span>

1. <span data-ttu-id="27293-109">Busque la línea `var app = express();` **en ./app.js**.</span><span class="sxs-lookup"><span data-stu-id="27293-109">Locate the line `var app = express();` in **./app.js**.</span></span> <span data-ttu-id="27293-110">Inserte el siguiente código después **de** esa línea.</span><span class="sxs-lookup"><span data-stu-id="27293-110">Insert the following code **after** that line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    <span data-ttu-id="27293-111">Este código inicializa la biblioteca msal-node con el identificador de aplicación y la contraseña de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="27293-111">This code initializes the msal-node library with the app ID and password for the app.</span></span>

1. <span data-ttu-id="27293-112">Cree un nuevo archivo en el directorio **./routes** denominado **auth.js** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="27293-112">Create a new file in the **./routes** directory named **auth.js** and add the following code.</span></span>

    ```javascript
    var router = require('express-promise-router')();

    /* GET auth callback. */
    router.get('/signin',
      async function (req, res) {
        const urlParameters = {
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const authUrl = await req.app.locals
            .msalClient.getAuthCodeUrl(urlParameters);
          res.redirect(authUrl);
        }
        catch (error) {
          console.log(`Error: ${error}`);
          req.flash('error_msg', {
            message: 'Error getting auth URL',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
          res.redirect('/');
        }
      }
    );

    router.get('/callback',
      async function(req, res) {
        const tokenRequest = {
          code: req.query.code,
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const response = await req.app.locals
            .msalClient.acquireTokenByCode(tokenRequest);

          // TEMPORARY!
          // Flash the access token for testing purposes
          req.flash('error_msg', {
            message: 'Access token',
            debug: response.accessToken
          });
        } catch (error) {
          req.flash('error_msg', {
            message: 'Error completing authentication',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
        }

        res.redirect('/');
      }
    );

    router.get('/signout',
      async function(req, res) {
        // Sign out
        if (req.session.userId) {
          // Look up the user's account in the cache
          const accounts = await req.app.locals.msalClient
            .getTokenCache()
            .getAllAccounts();

          const userAccount = accounts.find(a => a.homeAccountId === req.session.userId);

          // Remove the account
          if (userAccount) {
            req.app.locals.msalClient
              .getTokenCache()
              .removeAccount(userAccount);
          }
        }

        // Destroy the user's session
        req.session.destroy(function (err) {
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    <span data-ttu-id="27293-113">Esto define un enrutador con tres rutas: `signin` `callback` , y `signout` .</span><span class="sxs-lookup"><span data-stu-id="27293-113">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="27293-114">La ruta llama a la función para generar la dirección URL de inicio de sesión y, a continuación, redirige `signin` el explorador a esa dirección `getAuthCodeUrl` URL.</span><span class="sxs-lookup"><span data-stu-id="27293-114">The `signin` route calls the `getAuthCodeUrl` function to generate the login URL, then redirects the browser to that URL.</span></span>

    <span data-ttu-id="27293-115">La ruta es donde Azure redirige una vez completado `callback` el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-115">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="27293-116">El código llama a la `acquireTokenByCode` función para intercambiar el código de autorización por un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="27293-116">The code calls the `acquireTokenByCode` function to exchange the authorization code for an access token.</span></span> <span data-ttu-id="27293-117">Una vez que se obtiene el token, vuelve a la página principal con el token de acceso en el valor de error temporal.</span><span class="sxs-lookup"><span data-stu-id="27293-117">Once the token is obtained, it redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="27293-118">Lo usaremos para comprobar que nuestro inicio de sesión funciona antes de seguir adelante.</span><span class="sxs-lookup"><span data-stu-id="27293-118">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="27293-119">Antes de probar, debemos configurar la aplicación Express para usar el nuevo enrutador de **./routes/auth.js**.</span><span class="sxs-lookup"><span data-stu-id="27293-119">Before we test, we need to configure the Express app to use the new router from **./routes/auth.js**.</span></span>

    <span data-ttu-id="27293-120">El `signout` método cierra la sesión del usuario y destruye la sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-120">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="27293-121">Abra **./app.js** e inserte el siguiente código **antes de** la `var app = express();` línea.</span><span class="sxs-lookup"><span data-stu-id="27293-121">Open **./app.js** and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="27293-122">Inserte el siguiente código **después de** la `app.use('/', indexRouter);` línea.</span><span class="sxs-lookup"><span data-stu-id="27293-122">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="27293-123">Inicie el servidor y vaya a `https://localhost:3000` .</span><span class="sxs-lookup"><span data-stu-id="27293-123">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="27293-124">Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com` redirigirá.</span><span class="sxs-lookup"><span data-stu-id="27293-124">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="27293-125">Inicie sesión con su cuenta de Microsoft y consiente los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="27293-125">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="27293-126">El explorador redirige a la aplicación mostrando el token.</span><span class="sxs-lookup"><span data-stu-id="27293-126">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="27293-127">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="27293-127">Get user details</span></span>

1. <span data-ttu-id="27293-128">Cree un nuevo archivo en la raíz del proyecto denominado **graph.js** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="27293-128">Create a new file in the root of the project named **graph.js** and add the following code.</span></span>

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client
          .api('/me')
          .select('displayName,mail,mailboxSettings,userPrincipalName')
          .get();
        return user;
      },
    };

    function getAuthenticatedClient(accessToken) {
      // Initialize Graph client
      const client = graph.Client.init({
        // Use the provided access token to authenticate
        // requests
        authProvider: (done) => {
          done(null, accessToken);
        }
      });

      return client;
    }
    ```

    <span data-ttu-id="27293-129">Esto exporta la función, que usa el SDK de Microsoft Graph para llamar al punto de `getUserDetails` conexión y devolver el `/me` resultado.</span><span class="sxs-lookup"><span data-stu-id="27293-129">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="27293-130">Abra **./routes/auth.js** y agregue las siguientes `require` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="27293-130">Open **./routes/auth.js** and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('../graph');
    ```

1. <span data-ttu-id="27293-131">Reemplace la ruta de devolución de llamada existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="27293-131">Replace the existing callback route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    <span data-ttu-id="27293-132">El nuevo código guarda el identificador de cuenta del usuario en la sesión, obtiene los detalles del usuario de Microsoft Graph y lo guarda en el almacenamiento del usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="27293-132">The new code saves the user's account ID in the session, gets the user's details from Microsoft Graph, and saves it in the app's user storage.</span></span>

1. <span data-ttu-id="27293-133">Reinicie el servidor y pase por el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-133">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="27293-134">Debería volver a la página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-134">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Captura de pantalla de la página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. <span data-ttu-id="27293-136">Haz clic en el avatar del usuario en la esquina superior derecha para acceder al vínculo **Cerrar** sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-136">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="27293-137">Al **hacer clic en** Cerrar sesión, se restablece la sesión y se vuelve a la página principal.</span><span class="sxs-lookup"><span data-stu-id="27293-137">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Captura de pantalla del menú desplegable con el vínculo Cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="27293-139">Almacenar y actualizar tokens</span><span class="sxs-lookup"><span data-stu-id="27293-139">Storing and refreshing tokens</span></span>

<span data-ttu-id="27293-140">En este momento, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas API.</span><span class="sxs-lookup"><span data-stu-id="27293-140">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="27293-141">Este es el token que permite que la aplicación acceda a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="27293-141">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="27293-142">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="27293-142">However, this token is short-lived.</span></span> <span data-ttu-id="27293-143">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="27293-143">The token expires an hour after it is issued.</span></span> <span data-ttu-id="27293-144">Aquí es donde el token de actualización resulta útil.</span><span class="sxs-lookup"><span data-stu-id="27293-144">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="27293-145">La especificación de OAuth presenta un token de actualización, que permite a la aplicación solicitar un nuevo token de acceso sin necesidad de que el usuario vuelva a iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="27293-145">The OAuth specification introduces a refresh token, which allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="27293-146">Dado que la aplicación usa el paquete msal-node, no es necesario implementar ningún almacenamiento de tokens ni lógica de actualización.</span><span class="sxs-lookup"><span data-stu-id="27293-146">Because the app is using the msal-node package, you do not need to implement any token storage or refresh logic.</span></span> <span data-ttu-id="27293-147">La aplicación usa la memoria caché de token en memoria msal-node predeterminada, que es suficiente para una aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="27293-147">The app uses the default msal-node in-memory token cache, which is sufficient for a sample application.</span></span> <span data-ttu-id="27293-148">Las aplicaciones de producción deben proporcionar su propio [complemento de almacenamiento en](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) caché para serializar la memoria caché de tokens en un medio de almacenamiento seguro y confiable.</span><span class="sxs-lookup"><span data-stu-id="27293-148">Production applications should provide their own [caching plugin](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) to serialize the token cache in a secure, reliable storage medium.</span></span>
