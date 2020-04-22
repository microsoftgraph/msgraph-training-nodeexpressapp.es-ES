<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="23f6c-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23f6c-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="23f6c-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="23f6c-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="23f6c-103">En este paso, integrará la biblioteca [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23f6c-103">In this step you will integrate the [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) library into the application.</span></span>

1. <span data-ttu-id="23f6c-104">Cree un nuevo archivo denominado `.env` archivo en la raíz de la aplicación y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="23f6c-104">Create a new file named `.env` file in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/.env.example":::

    <span data-ttu-id="23f6c-105">Reemplace `YOUR APP ID HERE` por el identificador de la aplicación del portal de registro de la `YOUR APP SECRET HERE` aplicación y reemplace por la contraseña que ha generado.</span><span class="sxs-lookup"><span data-stu-id="23f6c-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="23f6c-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `.env` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="23f6c-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="23f6c-107">Abra `./app.js` y agregue la siguiente línea en la parte superior del archivo para cargar el `.env` archivo.</span><span class="sxs-lookup"><span data-stu-id="23f6c-107">Open `./app.js` and add the following line to the top of the file to load the `.env` file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="23f6c-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="23f6c-108">Implement sign-in</span></span>

1. <span data-ttu-id="23f6c-109">Busque la línea `var indexRouter = require('./routes/index');` en `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="23f6c-109">Locate the line `var indexRouter = require('./routes/index');` in `./app.js`.</span></span> <span data-ttu-id="23f6c-110">Inserte el siguiente código **antes** de esa línea.</span><span class="sxs-lookup"><span data-stu-id="23f6c-110">Insert the following code **before** that line.</span></span>

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

    <span data-ttu-id="23f6c-111">Este código inicializa la biblioteca [Passport. js](http://www.passportjs.org/) para usar la `passport-azure-ad` biblioteca y la configura con el identificador de aplicación y la contraseña de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23f6c-111">This code initializes the [Passport.js](http://www.passportjs.org/) library to use the `passport-azure-ad` library, and configures it with the app ID and password for the app.</span></span>

1. <span data-ttu-id="23f6c-112">Busque la línea `app.use('/', indexRouter);` en `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="23f6c-112">Locate the line `app.use('/', indexRouter);` in `./app.js`.</span></span> <span data-ttu-id="23f6c-113">Inserte el siguiente código **antes** de esa línea.</span><span class="sxs-lookup"><span data-stu-id="23f6c-113">Insert the following code **before** that line.</span></span>

    ```javascript
    // Initialize passport
    app.use(passport.initialize());
    app.use(passport.session());
    ```

1. <span data-ttu-id="23f6c-114">Cree un nuevo archivo en el `./routes` directorio denominado `auth.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="23f6c-114">Create a new file in the `./routes` directory named `auth.js` and add the following code.</span></span>

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

    <span data-ttu-id="23f6c-115">Define un enrutador con tres rutas `signin`: `callback`, y `signout`.</span><span class="sxs-lookup"><span data-stu-id="23f6c-115">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="23f6c-116">La `signin` ruta llama al `passport.authenticate` método, lo que hace que la aplicación se redirija a la página de inicio de sesión de Azure.</span><span class="sxs-lookup"><span data-stu-id="23f6c-116">The `signin` route calls the `passport.authenticate` method, causing the app to redirect to the Azure login page.</span></span>

    <span data-ttu-id="23f6c-117">La `callback` ruta es donde Azure redirige después de que se complete el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="23f6c-117">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="23f6c-118">El código llama al `passport.authenticate` método de nuevo, lo `passport-azure-ad` que hace que la estrategia solicite un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="23f6c-118">The code calls the `passport.authenticate` method again, causing the `passport-azure-ad` strategy to request an access token.</span></span> <span data-ttu-id="23f6c-119">Una vez que se obtiene el token, se llama al siguiente controlador, que redirige de nuevo a la Página principal con el token de acceso en el valor de error temporal.</span><span class="sxs-lookup"><span data-stu-id="23f6c-119">Once the token is obtained, the next handler is called, which redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="23f6c-120">Usaremos esto para comprobar que el inicio de sesión está funcionando antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="23f6c-120">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="23f6c-121">Antes de realizar las pruebas, es necesario configurar la aplicación Express para que use el nuevo `./routes/auth.js`enrutador de.</span><span class="sxs-lookup"><span data-stu-id="23f6c-121">Before we test, we need to configure the Express app to use the new router from `./routes/auth.js`.</span></span>

    <span data-ttu-id="23f6c-122">El `signout` método cierra la sesión del usuario y destruye la sesión.</span><span class="sxs-lookup"><span data-stu-id="23f6c-122">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="23f6c-123">Abra `./app.js` e inserte el siguiente código **antes** de `var app = express();` la línea.</span><span class="sxs-lookup"><span data-stu-id="23f6c-123">Open `./app.js` and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="23f6c-124">Inserte el siguiente código **después** de `app.use('/', indexRouter);` la línea.</span><span class="sxs-lookup"><span data-stu-id="23f6c-124">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="23f6c-125">Inicie el servidor y vaya a `https://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="23f6c-125">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="23f6c-126">Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com`redirigirá a.</span><span class="sxs-lookup"><span data-stu-id="23f6c-126">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="23f6c-127">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="23f6c-127">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="23f6c-128">El explorador redirige a la aplicación, que muestra el token.</span><span class="sxs-lookup"><span data-stu-id="23f6c-128">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="23f6c-129">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="23f6c-129">Get user details</span></span>

1. <span data-ttu-id="23f6c-130">Cree un nuevo archivo en la raíz del proyecto denominado `graph.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="23f6c-130">Create a new file in the root of the project named `graph.js` and add the following code.</span></span>

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

    <span data-ttu-id="23f6c-131">De esta forma `getUserDetails` , se exporta la función, que usa el SDK de `/me` Microsoft Graph para llamar al punto de conexión y devolver el resultado.</span><span class="sxs-lookup"><span data-stu-id="23f6c-131">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="23f6c-132">Abra `/app.js` y agregue las siguientes `require` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="23f6c-132">Open `/app.js` and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('./graph');
    ```

1. <span data-ttu-id="23f6c-133">Reemplace la función `signInComplete` existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="23f6c-133">Replace the existing `signInComplete` function with the following code.</span></span>

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

    <span data-ttu-id="23f6c-134">El nuevo código actualiza el `profile` proporcionado por Passport para agregar una `email` propiedad usando los datos devueltos por Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="23f6c-134">The new code updates the `profile` provided by Passport to add an `email` property, using the data returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="23f6c-135">Agregue lo siguiente **después** de `app.use(passport.session());` la línea.</span><span class="sxs-lookup"><span data-stu-id="23f6c-135">Add the following **after** the `app.use(passport.session());` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="AddProfileSnippet":::

    <span data-ttu-id="23f6c-136">Este código carga el perfil de usuario en `locals` la propiedad de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="23f6c-136">This code loads the user profile into the `locals` property of the response.</span></span> <span data-ttu-id="23f6c-137">Esto hará que esté disponible para todas las vistas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23f6c-137">This will make it available to all of the views in the app.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="23f6c-138">Almacenamiento de tokens</span><span class="sxs-lookup"><span data-stu-id="23f6c-138">Storing the tokens</span></span>

<span data-ttu-id="23f6c-139">Ahora que puede obtener tokens, es el momento de implementar una forma de almacenarlos en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23f6c-139">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="23f6c-140">Actualmente, la aplicación está almacenando el token de acceso sin formato en el almacenamiento de usuarios en memoria.</span><span class="sxs-lookup"><span data-stu-id="23f6c-140">Currently, the app is storing the raw access token in the in-memory user storage.</span></span> <span data-ttu-id="23f6c-141">Dado que se trata de una aplicación de ejemplo, por razones de simplicidad, podrá seguir almacenando allí.</span><span class="sxs-lookup"><span data-stu-id="23f6c-141">Since this is a sample app, for simplicity's sake, you'll continue to store them there.</span></span> <span data-ttu-id="23f6c-142">Una aplicación real usaría una solución de almacenamiento seguro más confiable, como una base de datos.</span><span class="sxs-lookup"><span data-stu-id="23f6c-142">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="23f6c-143">Sin embargo, el almacenamiento solo del token de acceso no permite comprobar la expiración ni actualizar el token.</span><span class="sxs-lookup"><span data-stu-id="23f6c-143">However, storing just the access token doesn't allow you to check expiration or refresh the token.</span></span> <span data-ttu-id="23f6c-144">Para habilitarlo, actualice el ejemplo para ajustar los tokens en un `AccessToken` objeto de la `simple-oauth2` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="23f6c-144">In order to enable that, update the sample to wrap the tokens in an `AccessToken` object from the `simple-oauth2` library.</span></span>

1. <span data-ttu-id="23f6c-145">Abra `./app.js` y agregue el código siguiente **antes** de `signInComplete` la función.</span><span class="sxs-lookup"><span data-stu-id="23f6c-145">Open `./app.js` and add the following code **before** the `signInComplete` function.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="ConfigureOAuth2Snippet":::

1. <span data-ttu-id="23f6c-146">Reemplace la función `signInComplete` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="23f6c-146">Replace the existing `signInComplete` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SignInCompleteSnippet" highlight="17-18, 21":::

1. <span data-ttu-id="23f6c-147">Reemplace la ruta de devolución de `./routes/auth.js` llamada existente con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="23f6c-147">Replace the existing callback route in  `./routes/auth.js` with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackRouteSnippet" highlight="17-18":::

1. <span data-ttu-id="23f6c-148">Reinicie el servidor y pase por el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="23f6c-148">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="23f6c-149">Deberás volver a la Página principal, pero la interfaz de usuario debe cambiar para indicar que has iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="23f6c-149">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. <span data-ttu-id="23f6c-151">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="23f6c-151">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="23f6c-152">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="23f6c-152">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="23f6c-154">Actualizar tokens</span><span class="sxs-lookup"><span data-stu-id="23f6c-154">Refreshing tokens</span></span>

<span data-ttu-id="23f6c-155">En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="23f6c-155">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="23f6c-156">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="23f6c-156">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="23f6c-157">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="23f6c-157">However, this token is short-lived.</span></span> <span data-ttu-id="23f6c-158">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="23f6c-158">The token expires an hour after it is issued.</span></span> <span data-ttu-id="23f6c-159">Aquí es donde el token de actualización se vuelve útil.</span><span class="sxs-lookup"><span data-stu-id="23f6c-159">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="23f6c-160">El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.</span><span class="sxs-lookup"><span data-stu-id="23f6c-160">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

1. <span data-ttu-id="23f6c-161">Cree un nuevo archivo en la raíz del proyecto denominado `tokens.js` para mantener las funciones de administración de tokens.</span><span class="sxs-lookup"><span data-stu-id="23f6c-161">Create a new file in the root of the project named `tokens.js` to hold token management functions.</span></span> <span data-ttu-id="23f6c-162">Agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="23f6c-162">Add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/tokens.js" id="TokensSnippet":::

<span data-ttu-id="23f6c-163">Este método comprueba primero si el token de acceso ha expirado o está próximo a expirar.</span><span class="sxs-lookup"><span data-stu-id="23f6c-163">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="23f6c-164">Si es así, usa el token de actualización para obtener nuevos tokens y, a continuación, actualiza la memoria caché y devuelve el nuevo token de acceso.</span><span class="sxs-lookup"><span data-stu-id="23f6c-164">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span> <span data-ttu-id="23f6c-165">Usaremos este método siempre que necesites obtener el token de acceso de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="23f6c-165">You'll use this method whenever you need to get the access token out of storage.</span></span>
