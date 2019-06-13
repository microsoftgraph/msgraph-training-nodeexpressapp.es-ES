<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3fa49-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3fa49-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="3fa49-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3fa49-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="3fa49-103">En este paso, integrará la biblioteca [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa49-103">In this step you will integrate the [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) library into the application.</span></span>

<span data-ttu-id="3fa49-104">Cree un nuevo archivo denominado `.env` archivo en la raíz de la aplicación y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3fa49-104">Create a new file named `.env` file in the root of your application, and add the following code.</span></span>

```text
OAUTH_APP_ID=YOUR_APP_ID_HERE
OAUTH_APP_PASSWORD=YOUR_APP_PASSWORD_HERE
OAUTH_REDIRECT_URI=http://localhost:3000/auth/callback
OAUTH_SCOPES='profile offline_access user.read calendars.read'
OAUTH_AUTHORITY=https://login.microsoftonline.com/common
OAUTH_ID_METADATA=/v2.0/.well-known/openid-configuration
OAUTH_AUTHORIZE_ENDPOINT=/oauth2/v2.0/authorize
OAUTH_TOKEN_ENDPOINT=/oauth2/v2.0/token
```

<span data-ttu-id="3fa49-105">Reemplace `YOUR APP ID HERE` por el identificador de la aplicación del portal de registro de la `YOUR APP SECRET HERE` aplicación y reemplace por la contraseña que ha generado.</span><span class="sxs-lookup"><span data-stu-id="3fa49-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3fa49-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `.env` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="3fa49-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

<span data-ttu-id="3fa49-107">Abra `./app.js` y agregue la siguiente línea en la parte superior del archivo para cargar el `.env` archivo.</span><span class="sxs-lookup"><span data-stu-id="3fa49-107">Open `./app.js` and add the following line to the top of the file to load the `.env` file.</span></span>

```js
require('dotenv').config();
```

## <a name="implement-sign-in"></a><span data-ttu-id="3fa49-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="3fa49-108">Implement sign-in</span></span>

<span data-ttu-id="3fa49-109">Busque la línea `var indexRouter = require('./routes/index');` en `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="3fa49-109">Locate the line `var indexRouter = require('./routes/index');` in `./app.js`.</span></span> <span data-ttu-id="3fa49-110">Inserte el siguiente código **antes** de esa línea.</span><span class="sxs-lookup"><span data-stu-id="3fa49-110">Insert the following code **before** that line.</span></span>

```js
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
    return done(new Error("No OID found in user profile."), null);
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

<span data-ttu-id="3fa49-111">Este código inicializa la biblioteca [Passport. js](http://www.passportjs.org/) para usar la `passport-azure-ad` biblioteca y la configura con el identificador de aplicación y la contraseña de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa49-111">This code initializes the [Passport.js](http://www.passportjs.org/) library to use the `passport-azure-ad` library, and configures it with the app ID and password for the app.</span></span>

<span data-ttu-id="3fa49-112">Ahora pase el `passport` objeto a la aplicación Express.</span><span class="sxs-lookup"><span data-stu-id="3fa49-112">Now pass the `passport` object to the Express app.</span></span> <span data-ttu-id="3fa49-113">Busque la línea `app.use('/', indexRouter);` en `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="3fa49-113">Locate the line `app.use('/', indexRouter);` in `./app.js`.</span></span> <span data-ttu-id="3fa49-114">Inserte el siguiente código **antes** de esa línea.</span><span class="sxs-lookup"><span data-stu-id="3fa49-114">Insert the following code **before** that line.</span></span>

```js
// Initialize passport
app.use(passport.initialize());
app.use(passport.session());
```

<span data-ttu-id="3fa49-115">Cree un nuevo archivo en el `./routes` directorio denominado `auth.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3fa49-115">Create a new file in the `./routes` directory named `auth.js` and add the following code.</span></span>

```js
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
        failureFlash: true
      }
    )(req,res,next);
  },
  function(req, res) {
    res.redirect('/');
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

<span data-ttu-id="3fa49-116">Define un enrutador con tres rutas `signin`: `callback`, y `signout`.</span><span class="sxs-lookup"><span data-stu-id="3fa49-116">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

<span data-ttu-id="3fa49-117">La `signin` ruta llama al `passport.authenticate` método, lo que hace que la aplicación se redirija a la página de inicio de sesión de Azure.</span><span class="sxs-lookup"><span data-stu-id="3fa49-117">The `signin` route calls the `passport.authenticate` method, causing the app to redirect to the Azure login page.</span></span>

<span data-ttu-id="3fa49-118">La `callback` ruta es donde Azure redirige después de que se complete el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa49-118">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="3fa49-119">El código llama al `passport.authenticate` método de nuevo, lo `passport-azure-ad` que hace que la estrategia solicite un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="3fa49-119">The code calls the `passport.authenticate` method again, causing the `passport-azure-ad` strategy to request an access token.</span></span> <span data-ttu-id="3fa49-120">Una vez que se obtiene el token, se llama al siguiente controlador, que redirige de nuevo a la Página principal con el token de acceso en el valor de error temporal.</span><span class="sxs-lookup"><span data-stu-id="3fa49-120">Once the token is obtained, the next handler is called, which redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="3fa49-121">Usaremos esto para comprobar que el inicio de sesión está funcionando antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="3fa49-121">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="3fa49-122">Antes de realizar las pruebas, es necesario configurar la aplicación Express para que use el nuevo `./routes/auth.js`enrutador de.</span><span class="sxs-lookup"><span data-stu-id="3fa49-122">Before we test, we need to configure the Express app to use the new router from `./routes/auth.js`.</span></span>

<span data-ttu-id="3fa49-123">El `signout` método cierra la sesión del usuario y destruye la sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa49-123">The `signout` method logs the user out and destroys the session.</span></span>

<span data-ttu-id="3fa49-124">Inserte el siguiente código **antes** de `var app = express();` la línea.</span><span class="sxs-lookup"><span data-stu-id="3fa49-124">Insert the following code **before** the `var app = express();` line.</span></span>

```js
var authRouter = require('./routes/auth');
```

<span data-ttu-id="3fa49-125">A continuación, inserte el \*\*\*\* siguiente código `app.use('/', indexRouter);` después de la línea.</span><span class="sxs-lookup"><span data-stu-id="3fa49-125">Then insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

```js
app.use('/auth', authRouter);
```

<span data-ttu-id="3fa49-126">Inicie el servidor y vaya a `https://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="3fa49-126">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="3fa49-127">Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com`redirigirá a.</span><span class="sxs-lookup"><span data-stu-id="3fa49-127">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="3fa49-128">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="3fa49-128">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="3fa49-129">El explorador redirige a la aplicación, que muestra el token.</span><span class="sxs-lookup"><span data-stu-id="3fa49-129">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="3fa49-130">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="3fa49-130">Get user details</span></span>

<span data-ttu-id="3fa49-131">Para empezar, cree un nuevo archivo que contenga todas las llamadas de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3fa49-131">Start by creating a new file to hold all of your Microsoft Graph calls.</span></span> <span data-ttu-id="3fa49-132">Cree un nuevo archivo en la raíz del proyecto denominado `graph.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3fa49-132">Create a new file in the root of the project named `graph.js` and add the following code.</span></span>

```js
var graph = require('@microsoft/microsoft-graph-client');

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

<span data-ttu-id="3fa49-133">De esta forma `getUserDetails` , se exporta la función, que usa el SDK de `/me` Microsoft Graph para llamar al punto de conexión y devolver el resultado.</span><span class="sxs-lookup"><span data-stu-id="3fa49-133">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

<span data-ttu-id="3fa49-134">Actualice el `signInComplete` método en `/app.s` para llamar a esta función.</span><span class="sxs-lookup"><span data-stu-id="3fa49-134">Update the `signInComplete` method in `/app.s` to call this function.</span></span> <span data-ttu-id="3fa49-135">En primer lugar, agregue `require` las siguientes instrucciones a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="3fa49-135">First, add the following `require` statements to the top of the file.</span></span>

```js
var graph = require('./graph');
```

<span data-ttu-id="3fa49-136">Reemplace la función `signInComplete` existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3fa49-136">Replace the existing `signInComplete` function with the following code.</span></span>

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, accessToken };
  return done(null, users[profile.oid]);
}
```

<span data-ttu-id="3fa49-137">El nuevo código actualiza el `profile` proporcionado por Passport para agregar una `email` propiedad usando los datos devueltos por Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3fa49-137">The new code updates the `profile` provided by Passport to add an `email` property, using the data returned by Microsoft Graph.</span></span>

<span data-ttu-id="3fa49-138">Por último, agregue código `./app.js` a para cargar el perfil de usuario `locals` en la propiedad de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="3fa49-138">Finally, add code to `./app.js` to load the user profile into the `locals` property of the response.</span></span> <span data-ttu-id="3fa49-139">Esto hará que esté disponible para todas las vistas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa49-139">This will make it available to all of the views in the app.</span></span>

<span data-ttu-id="3fa49-140">Agregue lo siguiente **después** de `app.use(passport.session());` la línea.</span><span class="sxs-lookup"><span data-stu-id="3fa49-140">Add the following **after** the `app.use(passport.session());` line.</span></span>

```js
app.use(function(req, res, next) {
  // Set the authenticated user in the
  // template locals
  if (req.user) {
    res.locals.user = req.user.profile;
  }
  next();
});
```

## <a name="storing-the-tokens"></a><span data-ttu-id="3fa49-141">Almacenamiento de tokens</span><span class="sxs-lookup"><span data-stu-id="3fa49-141">Storing the tokens</span></span>

<span data-ttu-id="3fa49-142">Ahora que puede obtener tokens, es el momento de implementar una forma de almacenarlos en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa49-142">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="3fa49-143">Actualmente, la aplicación está almacenando el token de acceso sin formato en el almacenamiento de usuarios en memoria.</span><span class="sxs-lookup"><span data-stu-id="3fa49-143">Currently, the app is storing the raw access token in the in-memory user storage.</span></span> <span data-ttu-id="3fa49-144">Dado que se trata de una aplicación de ejemplo, por razones de simplicidad, podrá seguir almacenando allí.</span><span class="sxs-lookup"><span data-stu-id="3fa49-144">Since this is a sample app, for simplicity's sake, you'll continue to store them there.</span></span> <span data-ttu-id="3fa49-145">Una aplicación real usaría una solución de almacenamiento seguro más confiable, como una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fa49-145">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="3fa49-146">Sin embargo, el almacenamiento solo del token de acceso no permite comprobar la expiración ni actualizar el token.</span><span class="sxs-lookup"><span data-stu-id="3fa49-146">However, storing just the access token doesn't allow you to check expiration or refresh the token.</span></span> <span data-ttu-id="3fa49-147">Para habilitarlo, actualice el ejemplo para ajustar los tokens en un `AccessToken` objeto de la `simple-oauth2` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="3fa49-147">In order to enable that, update the sample to wrap the tokens in an `AccessToken` object from the `simple-oauth2` library.</span></span>

<span data-ttu-id="3fa49-148">En primer lugar `./app.js`, en, agregue el siguiente código `signInComplete` **antes** de la función.</span><span class="sxs-lookup"><span data-stu-id="3fa49-148">First, in `./app.js`, add the following code **before** the `signInComplete` function.</span></span>

```js
// Configure simple-oauth2
const oauth2 = require('simple-oauth2').create({
  client: {
    id: process.env.OAUTH_APP_ID,
    secret: process.env.OAUTH_APP_PASSWORD
  },
  auth: {
    tokenHost: process.env.OAUTH_AUTHORITY,
    authorizePath: process.env.OAUTH_AUTHORIZE_ENDPOINT,
    tokenPath: process.env.OAUTH_TOKEN_ENDPOINT
  }
});
```

<span data-ttu-id="3fa49-149">A continuación, actualice `signInComplete` la función para crear `AccessToken` un a partir de los tokens sin formato pasados y almacenarlos en el almacenamiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="3fa49-149">Then, update the `signInComplete` function to create an `AccessToken` from the raw tokens passed in and store that in the user storage.</span></span> <span data-ttu-id="3fa49-150">Reemplace la función `signInComplete` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="3fa49-150">Replace the existing `signInComplete` function with the following.</span></span>

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Create a simple-oauth2 token from raw tokens
  let oauthToken = oauth2.accessToken.create(params);

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, oauthToken };
  return done(null, users[profile.oid]);
}
```

<span data-ttu-id="3fa49-151">Actualice la `callback` ruta en `./routes/auth.js` para quitar la `req.flash` línea con el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="3fa49-151">Update the `callback` route in `./routes/auth.js` to remove the `req.flash` line with the access token.</span></span> <span data-ttu-id="3fa49-152">La `callback` ruta debe tener un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="3fa49-152">The `callback` route should look like the following.</span></span>

```js
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
    res.redirect('/');
  }
);
```

<span data-ttu-id="3fa49-153">Reinicie el servidor y pase por el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa49-153">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="3fa49-154">Deberás volver a la Página principal, pero la interfaz de usuario debe cambiar para indicar que has iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa49-154">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

<span data-ttu-id="3fa49-156">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="3fa49-156">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="3fa49-157">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="3fa49-157">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="3fa49-159">Actualizar tokens</span><span class="sxs-lookup"><span data-stu-id="3fa49-159">Refreshing tokens</span></span>

<span data-ttu-id="3fa49-160">En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="3fa49-160">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="3fa49-161">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="3fa49-161">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="3fa49-162">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="3fa49-162">However, this token is short-lived.</span></span> <span data-ttu-id="3fa49-163">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="3fa49-163">The token expires an hour after it is issued.</span></span> <span data-ttu-id="3fa49-164">Aquí es donde el token de actualización se vuelve útil.</span><span class="sxs-lookup"><span data-stu-id="3fa49-164">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="3fa49-165">El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.</span><span class="sxs-lookup"><span data-stu-id="3fa49-165">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="3fa49-166">Para administrar esto, cree un nuevo archivo en la raíz del proyecto denominado `tokens.js` para mantener las funciones de administración de tokens.</span><span class="sxs-lookup"><span data-stu-id="3fa49-166">To manage this, create a new file in the root of the project named `tokens.js` to hold token management functions.</span></span> <span data-ttu-id="3fa49-167">Agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3fa49-167">Add the following code.</span></span>

```js
module.exports = {
  getAccessToken: async function(req) {
    if (req.user) {
      // Get the stored token
      var storedToken = req.user.oauthToken;

      if (storedToken) {
        if (storedToken.expired()) {
          // refresh token
          var newToken = await storedToken.refresh();

          // Update stored token
          req.user.oauthToken = newToken;
          return newToken.token.access_token;
        }

        // Token still valid, just return it
        return storedToken.token.access_token;
      }
    }
  }
};
```

<span data-ttu-id="3fa49-168">Este método comprueba primero si el token de acceso ha expirado o está próximo a expirar.</span><span class="sxs-lookup"><span data-stu-id="3fa49-168">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="3fa49-169">Si es así, usa el token de actualización para obtener nuevos tokens y, a continuación, actualiza la memoria caché y devuelve el nuevo token de acceso.</span><span class="sxs-lookup"><span data-stu-id="3fa49-169">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span> <span data-ttu-id="3fa49-170">Usaremos este método siempre que necesites obtener el token de acceso de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="3fa49-170">You'll use this method whenever you need to get the access token out of storage.</span></span>
