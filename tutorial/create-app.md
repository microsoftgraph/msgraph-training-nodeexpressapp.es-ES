<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, usará [Express](http://expressjs.com/) para crear una aplicación Web. Si aún no tiene instalado Express generator, puede instalarlo desde la interfaz de línea de comandos (CLI) con el siguiente comando.

```Shell
npm install express-generator -g
```

Abra la CLI, vaya a un directorio donde tenga derechos para crear archivos y ejecute el siguiente comando para crear una nueva aplicación Express que use manillares [](http://handlebarsjs.com/) como motor de representación.

```Shell
express --hbs graph-tutorial
```

El generador de Express crea un nuevo directorio `graph-tutorial` al que se llama y scaffolding una aplicación Express. Navegue hasta este nuevo directorio y escriba el siguiente comando para instalar las dependencias.

```Shell
npm install
```

Una vez que haya finalizado el comando, use el siguiente comando para iniciar un servidor Web local.

```Shell
npm start
```

Abra el explorador y vaya a `http://localhost:3000`. Si todo funciona, verá un mensaje "Bienvenido a exprés". Si no ve ese mensaje, consulte la [Guía de introducción rápida](http://expressjs.com/starter/generator.html).

Antes de continuar, instale algunas gemas adicionales que usará más adelante:

- [dotenv](https://github.com/motdotla/dotenv) para cargar los valores de un archivo. env.
- [momento](https://github.com/moment/moment/) para dar formato a valores de fecha y hora.
- mensajes de error de [Connect-Flash](https://github.com/jaredhanson/connect-flash) a Flash en la aplicación.
- [Express-Session](https://github.com/expressjs/session) para almacenar valores en una sesión del lado servidor de la memoria.
- [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) para autenticar y obtener tokens de acceso.
- [simple: OAuth2 para la](https://github.com/lelylan/simple-oauth2) administración de tokens.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

Ejecute el siguiente comando en su CLI.

```Shell
npm install dotenv@6.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.15.6
npm install passport-azure-ad@4.0.0 simple-oauth2@2.2.1 @microsoft/microsoft-graph-client@1.5.2
```

>__USUARIOS DE WINDOWS__
>
>Si recibe el siguiente mensaje de error al intentar instalar estos paquetes:
>
> `gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.`
>
>Ejecute el siguiente comando para instalar las herramientas de compilación de Windows mediante una ventana de terminal con privilegios elevados (Administrador) que instala las herramientas de compilación de VS y también Python.
>
> `npm install --global --production windows-build-tools`

Ahora, actualice la aplicación para usar `connect-flash` el `express-session` software intermedio y. Abra el `./app.js` archivo y agregue la siguiente `require` instrucción en la parte superior del archivo.

```js
var session = require('express-session');
var flash = require('connect-flash');
```

Agregue el siguiente código inmediatamente después de `var app = express();` la línea.

```js
// Session middleware
// NOTE: Uses default in-memory session store, which is not
// suitable for production
app.use(session({
  secret: 'your_secret_value_here',
  resave: false,
  saveUninitialized: false,
  unset: 'destroy'
}));

// Flash middleware
app.use(flash());

// Set up local vars for template layout
app.use(function(req, res, next) {
  // Read any flashed errors and save
  // in the response locals
  res.locals.error = req.flash('error_msg');

  // Check for simple error string and
  // convert to layout's expected format
  var errs = req.flash('error');
  for (var i in errs){
    res.locals.error.push({message: 'An error occurred', debug: errs[i]});
  }

  next();
});
```

## <a name="design-the-app"></a>Diseñar la aplicación

Empiece por crear el diseño global para la aplicación. Abra el `./views/layout.hbs` archivo y reemplace todo el contenido por el código siguiente.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Node.js Graph Tutorial</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css"
      integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
      integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
      integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"
      integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
  </head>

  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container">
        <a href="/" class="navbar-brand">Node.js Graph Tutorial</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse"
          aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a href="/" class="nav-link{{#if active.home}} active{{/if}}">Home</a>
            </li>
            {{#if user}}
              <li class="nav-item" data-turbolinks="false">
                <a href="/calendar" class="nav-link{{#if active.calendar}} active{{/if}}">Calendar</a>
              </li>
            {{/if}}
          </ul>
          <ul class="navbar-nav justify-content-end">
            <li class="nav-item">
              <a class="nav-link" href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                <i class="fas fa-external-link-alt mr-1"></i>Docs
              </a>
            </li>
            {{#if user}}
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true"
                  aria-expanded="false">
                  {{#if user.avatar}}
                    <img src="{{ user.avatar }}" class="rounded-circle align-self-center mr-2" style="width: 32px;">
                  {{else}}
                    <i class="far fa-user-circle fa-lg rounded-circle align-self-center mr-2" style="width: 32px;"></i>
                  {{/if}}
                </a>
                <div class="dropdown-menu dropdown-menu-right">
                  <h5 class="dropdown-item-text mb-0">{{ user.displayName }}</h5>
                  <p class="dropdown-item-text text-muted mb-0">{{ user.email }}</p>
                  <div class="dropdown-divider"></div>
                  <a href="/auth/signout" class="dropdown-item">Sign Out</a>
                </div>
              </li>
            {{else}}
              <li class="nav-item">
                <a href="/auth/signin" class="nav-link">Sign In</a>
              </li>
            {{/if}}
          </ul>
        </div>
      </div>
    </nav>
    <main role="main" class="container">
      {{#each error}}
        <div class="alert alert-danger" role="alert">
          <p class="mb-3">{{ this.message }}</p>
          {{#if this.debug }}
            <pre class="alert-pre border bg-light p-2"><code>{{ this.debug }}</code></pre>
          {{/if}}
        </div>
      {{/each}}

      {{{body}}}
    </main>
  </body>
</html>
```

Este código agrega un [bootstrap](http://getbootstrap.com/) para los estilos sencillos y la [fuente maravilla](https://fontawesome.com/) para algunos iconos simples. También define un diseño global con una barra de navegación.

Ahora, `./public/stylesheets/style.css` abra y reemplace todo el contenido por lo siguiente.

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

Ahora, actualice la página predeterminada. Abra el `./views/index.hbs` archivo y reemplace el contenido por lo siguiente.

```html
<div class="jumbotron">
  <h1>Node.js Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from Node.js</p>
  {{#if user}}
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  {{else}}
    <a href="/auth/signin" class="btn btn-primary btn-large">Click here to sign in</a>
  {{/if}}
</div>
```

Abra el `./routes/index.js` archivo y reemplace el código existente por lo siguiente.

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  let params = {
    active: { home: true }
  };

  res.render('index', params);
});

module.exports = router;
```

Guarde todos los cambios y reinicie el servidor. Ahora, la aplicación debe tener un aspecto muy diferente.

![Una captura de pantalla de la Página principal rediseñada](./images/create-app-01.png)