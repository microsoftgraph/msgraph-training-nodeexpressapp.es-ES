<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="298de-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="298de-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="298de-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="298de-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="298de-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="298de-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="298de-104">Abra `./graph.js` y agregue la siguiente función dentro `module.exports`de.</span><span class="sxs-lookup"><span data-stu-id="298de-104">Open `./graph.js` and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetEventsSnippet":::

    <span data-ttu-id="298de-105">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="298de-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="298de-106">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="298de-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="298de-107">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="298de-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="298de-108">El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="298de-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="298de-109">Cree un nuevo archivo en el `./routes` directorio denominado `calendar.js`y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="298de-109">Create a new file in the `./routes` directory named `calendar.js`, and add the following code.</span></span>

    ```javascript
    var express = require('express');
    var router = express.Router();
    var tokens = require('../tokens.js');
    var graph = require('../graph.js');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.isAuthenticated()) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          let params = {
            active: { calendar: true }
          };

          // Get the access token
          var accessToken;
          try {
            accessToken = await tokens.getAccessToken(req);
          } catch (err) {
            res.json(err);
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              var events = await graph.getEvents(accessToken);

              res.json(events.value);
            } catch (err) {
              res.json(err);
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    module.exports = router;
    ```

1. <span data-ttu-id="298de-110">Actualice `./app.js` para usar esta nueva ruta.</span><span class="sxs-lookup"><span data-stu-id="298de-110">Update `./app.js` to use this new route.</span></span> <span data-ttu-id="298de-111">Agregue la siguiente línea **antes** de `var app = express();` la línea.</span><span class="sxs-lookup"><span data-stu-id="298de-111">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="298de-112">Agregue la siguiente línea **después** de `app.use('/auth', authRouter);` la línea.</span><span class="sxs-lookup"><span data-stu-id="298de-112">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="298de-113">Reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="298de-113">Restart the server.</span></span> <span data-ttu-id="298de-114">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="298de-114">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="298de-115">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="298de-115">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="298de-116">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="298de-116">Display the results</span></span>

<span data-ttu-id="298de-117">Ahora puede Agregar una vista para mostrar los resultados de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="298de-117">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="298de-118">Agregue el siguiente código en `./app.js` **después** de `app.set('view engine', 'hbs');` la línea.</span><span class="sxs-lookup"><span data-stu-id="298de-118">Add the following code in `./app.js` **after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="298de-119">Esto implementa una [aplicación auxiliar de manillar](http://handlebarsjs.com/#helpers) para dar formato a la fecha ISO 8601 devuelta por Microsoft Graph en algo más fácil de ser humano.</span><span class="sxs-lookup"><span data-stu-id="298de-119">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="298de-120">Cree un nuevo archivo en el `./views` directorio denominado `calendar.hbs` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="298de-120">Create a new file in the `./views` directory named `calendar.hbs` and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="298de-121">Se recorrerá en bucle una colección de eventos y se agregará una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="298de-121">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="298de-122">Ahora, actualice la ruta `./routes/calendar.js` en para usar esta vista.</span><span class="sxs-lookup"><span data-stu-id="298de-122">Now update the route in `./routes/calendar.js` to use this view.</span></span> <span data-ttu-id="298de-123">Reemplace la ruta existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="298de-123">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="16-19,26,28-31,37":::

1. <span data-ttu-id="298de-124">Guarde los cambios, reinicie el servidor e inicie sesión en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="298de-124">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="298de-125">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="298de-125">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
