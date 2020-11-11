<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la biblioteca [de nodos de msal](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) en la aplicación.

1. Cree un nuevo archivo denominado **. env** en la raíz de la aplicación y agregue el código siguiente.

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    Reemplace `YOUR_CLIENT_SECRET_HERE` por el identificador de aplicación del portal de registro de aplicaciones y reemplace `YOUR_APP_SECRET_HERE` por el secreto de cliente que ha generado.

    > [!IMPORTANT]
    > Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **. env** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación y la contraseña.

1. Abra **./app.js** y agregue la siguiente línea en la parte superior del archivo para cargar el archivo **. env** .

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

1. Busque la línea `var indexRouter = require('./routes/index');` in **./app.js**. Inserte el siguiente código **antes** de esa línea.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    Este código inicializa la biblioteca de nodos de msal con el identificador de la aplicación y la contraseña de la aplicación.

1. Cree un nuevo archivo en el directorio **./Routes** denominado **auth.js** y agregue el siguiente código.

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

    Define un enrutador con tres rutas: `signin` , `callback` y `signout` .

    La `signin` ruta llama `getAuthCodeUrl` a la función para generar la dirección URL de inicio de sesión y, a continuación, redirige el explorador a esa dirección URL.

    La `callback` ruta es donde Azure redirige después de que se complete el inicio de sesión. El código llama `acquireTokenByCode` a la función para intercambiar el código de autorización para un token de acceso. Una vez que se obtiene el token, se redirige de vuelta a la Página principal con el token de acceso en el valor de error temporal. Usaremos esto para comprobar que el inicio de sesión está funcionando antes de continuar. Antes de realizar las pruebas, es necesario configurar la aplicación Express para que use el nuevo enrutador de **./routes/auth.js**.

    El `signout` método cierra la sesión del usuario y destruye la sesión.

1. Abra **./app.js** e inserte el siguiente código **antes** de la `var app = express();` línea.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Inserte el siguiente código **después** de la `app.use('/', indexRouter);` línea.

    ```javascript
    app.use('/auth', authRouter);
    ```

Inicie el servidor y vaya a `https://localhost:3000` . Haga clic en el botón de inicio de sesión y se le redirigirá a `https://login.microsoftonline.com` . Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados. El explorador redirige a la aplicación, que muestra el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo archivo en la raíz del proyecto denominado **graph.js** y agregue el código siguiente.

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

    De esta forma, se exporta la `getUserDetails` función, que usa el SDK de Microsoft Graph para llamar al `/me` punto de conexión y devolver el resultado.

1. Abra el **auth.js./Routes/** y agregue las siguientes `require` instrucciones en la parte superior del archivo.

    ```javascript
    var graph = require('../graph');
    ```

1. Reemplace la ruta de devolución de llamada existente por el código siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    El nuevo código guarda el identificador de cuenta del usuario en la sesión, obtiene los detalles del usuario de Microsoft Graph y lo guarda en el almacenamiento del usuario de la aplicación.

1. Reinicie el servidor y pase por el proceso de inicio de sesión. Deberás volver a la Página principal, pero la interfaz de usuario debe cambiar para indicar que has iniciado sesión.

    ![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** . Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.

    ![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenamiento y actualización de tokens

En este punto, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas a la API. Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Aquí es donde el token de actualización se vuelve útil. La especificación OAuth presenta un token de actualización, que permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.

Debido a que la aplicación usa el paquete de nodo de msal, no es necesario implementar ninguna lógica de almacenamiento o actualización de tokens. La aplicación usa la memoria caché de token en memoria de msal predeterminada, que es suficiente para una aplicación de ejemplo. Las aplicaciones de producción deben proporcionar su propio [complemento de almacenamiento en caché](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) para serializar la caché de tokens en un medio de almacenamiento seguro y confiable.
