<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la biblioteca [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) en la aplicación.

1. Cree un nuevo archivo denominado `.env` archivo en la raíz de la aplicación y agregue el siguiente código.

    :::code language="ini" source="../demo/graph-tutorial/.env.example":::

    Reemplace `YOUR APP ID HERE` por el identificador de la aplicación del portal de registro de la `YOUR APP SECRET HERE` aplicación y reemplace por la contraseña que ha generado.

    > [!IMPORTANT]
    > Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `.env` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación y la contraseña.

1. Abra `./app.js` y agregue la siguiente línea en la parte superior del archivo para cargar el `.env` archivo.

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

1. Busque la línea `var indexRouter = require('./routes/index');` en `./app.js`. Inserte el siguiente código **antes** de esa línea.

    ```javascript
    var passport = require('passport');
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // Configure passport

    // In-memory storage of logged-in users
    // For demo purposes only, production apps should store
    // this in a reliable storage
    var users = {};

    // Passport calls serializeUser and deserializeUser to
    // manage users
    passport.serializeUser(function(user, done) {
      // Use the OID property of the user as a key
      users[user.profile.oid] = user;
      done (null, user.profile.oid);
    });

    passport.deserializeUser(function(id, done) {
      done(null, users[id]);
    });

    // Callback function called once the sign-in is complete
    // and an access token has been obtained
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }

    // Configure OIDC strategy
    passport.use(new OIDCStrategy(
      {
        identityMetadata: `${process.env.OAUTH_AUTHORITY}${process.env.OAUTH_ID_METADATA}`,
        clientID: process.env.OAUTH_APP_ID,
        responseType: 'code id_token',
        responseMode: 'form_post',
        redirectUrl: process.env.OAUTH_REDIRECT_URI,
        allowHttpForRedirectUrl: true,
        clientSecret: process.env.OAUTH_APP_PASSWORD,
        validateIssuer: false,
        passReqToCallback: false,
        scope: process.env.OAUTH_SCOPES.split(' ')
      },
      signInComplete
    ));
    ```

    Este código inicializa la biblioteca [Passport. js](http://www.passportjs.org/) para usar la `passport-azure-ad` biblioteca y la configura con el identificador de aplicación y la contraseña de la aplicación.

1. Busque la línea `app.use('/', indexRouter);` en `./app.js`. Inserte el siguiente código **antes** de esa línea.

    ```javascript
    // Initialize passport
    app.use(passport.initialize());
    app.use(passport.session());
    ```

1. Cree un nuevo archivo en el `./routes` directorio denominado `auth.js` y agregue el siguiente código.

    ```javascript
    var express = require('express');
    var passport = require('passport');
    var router = express.Router();

    /* GET auth callback. */
    router.get('/signin',
      function  (req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            prompt: 'login',
            failureRedirect: '/',
            failureFlash: true,
            successRedirect: '/'
          }
        )(req,res,next);
      }
    );

    router.post('/callback',
      function(req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            failureRedirect: '/',
            failureFlash: true
          }
        )(req,res,next);
      },
      function(req, res) {
        // TEMPORARY!
        // Flash the access token for testing purposes
        req.flash('error_msg', {message: 'Access token', debug: req.user.accessToken});
        res.redirect('/');
      }
    );

    router.get('/signout',
      function(req, res) {
        req.session.destroy(function(err) {
          req.logout();
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    Define un enrutador con tres rutas `signin`: `callback`, y `signout`.

    La `signin` ruta llama al `passport.authenticate` método, lo que hace que la aplicación se redirija a la página de inicio de sesión de Azure.

    La `callback` ruta es donde Azure redirige después de que se complete el inicio de sesión. El código llama al `passport.authenticate` método de nuevo, lo `passport-azure-ad` que hace que la estrategia solicite un token de acceso. Una vez que se obtiene el token, se llama al siguiente controlador, que redirige de nuevo a la Página principal con el token de acceso en el valor de error temporal. Usaremos esto para comprobar que el inicio de sesión está funcionando antes de continuar. Antes de realizar las pruebas, es necesario configurar la aplicación Express para que use el nuevo `./routes/auth.js`enrutador de.

    El `signout` método cierra la sesión del usuario y destruye la sesión.

1. Abra `./app.js` e inserte el siguiente código **antes** de `var app = express();` la línea.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Inserte el siguiente código **después** de `app.use('/', indexRouter);` la línea.

    ```javascript
    app.use('/auth', authRouter);
    ```

Inicie el servidor y vaya a `https://localhost:3000`. Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com`redirigirá a. Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados. El explorador redirige a la aplicación, que muestra el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo archivo en la raíz del proyecto denominado `graph.js` y agregue el siguiente código.

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client.api('/me').get();
        return user;
      }
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

    De esta forma `getUserDetails` , se exporta la función, que usa el SDK de `/me` Microsoft Graph para llamar al punto de conexión y devolver el resultado.

1. Abra `/app.js` y agregue las siguientes `require` instrucciones en la parte superior del archivo.

    ```javascript
    var graph = require('./graph');
    ```

1. Reemplace la función `signInComplete` existente por el siguiente código.

    ```javascript
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      try{
        const user = await graph.getUserDetails(accessToken);

        if (user) {
          // Add properties to profile
          profile['email'] = user.mail ? user.mail : user.userPrincipalName;
        }
      } catch (err) {
        return done(err);
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }
    ```

    El nuevo código actualiza el `profile` proporcionado por Passport para agregar una `email` propiedad usando los datos devueltos por Microsoft Graph.

1. Agregue lo siguiente **después** de `app.use(passport.session());` la línea.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="AddProfileSnippet":::

    Este código carga el perfil de usuario en `locals` la propiedad de la respuesta. Esto hará que esté disponible para todas las vistas de la aplicación.

## <a name="storing-the-tokens"></a>Almacenamiento de tokens

Ahora que puede obtener tokens, es el momento de implementar una forma de almacenarlos en la aplicación. Actualmente, la aplicación está almacenando el token de acceso sin formato en el almacenamiento de usuarios en memoria. Dado que se trata de una aplicación de ejemplo, por razones de simplicidad, podrá seguir almacenando allí. Una aplicación real usaría una solución de almacenamiento seguro más confiable, como una base de datos.

Sin embargo, el almacenamiento solo del token de acceso no permite comprobar la expiración ni actualizar el token. Para habilitarlo, actualice el ejemplo para ajustar los tokens en un `AccessToken` objeto de la `simple-oauth2` biblioteca.

1. Abra `./app.js` y agregue el código siguiente **antes** de `signInComplete` la función.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="ConfigureOAuth2Snippet":::

1. Reemplace la función `signInComplete` existente por lo siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SignInCompleteSnippet" highlight="17-18, 21":::

1. Reemplace la ruta de devolución de `./routes/auth.js` llamada existente con lo siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackRouteSnippet" highlight="17-18":::

1. Reinicie el servidor y pase por el proceso de inicio de sesión. Deberás volver a la Página principal, pero la interfaz de usuario debe cambiar para indicar que has iniciado sesión.

    ![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** . Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.

    ![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>Actualizar tokens

En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API. Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Aquí es donde el token de actualización se vuelve útil. El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.

1. Cree un nuevo archivo en la raíz del proyecto denominado `tokens.js` para mantener las funciones de administración de tokens. Agregue el siguiente código.

    :::code language="javascript" source="../demo/graph-tutorial/tokens.js" id="TokensSnippet":::

Este método comprueba primero si el token de acceso ha expirado o está próximo a expirar. Si es así, usa el token de actualización para obtener nuevos tokens y, a continuación, actualiza la memoria caché y devuelve el nuevo token de acceso. Usaremos este método siempre que necesites obtener el token de acceso de almacenamiento.
