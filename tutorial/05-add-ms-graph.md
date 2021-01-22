<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="58653-101">En este ejercicio incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58653-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="58653-102">Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="58653-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="58653-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="58653-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="58653-104">Abra **./graph.js** y agregue la siguiente función `module.exports` dentro.</span><span class="sxs-lookup"><span data-stu-id="58653-104">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    <span data-ttu-id="58653-105">Ten en cuenta lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="58653-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="58653-106">La dirección URL a la que se llamará es `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="58653-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="58653-107">El método agrega el encabezado a la solicitud, lo que provoca que las horas de inicio y finalización se devuelvan en la `header` `Prefer: outlook.timezone` zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="58653-107">The `header` method adds the `Prefer: outlook.timezone` header to the request, causing the start and end times to be returned in the user's time zone.</span></span>
    - <span data-ttu-id="58653-108">El `query` método establece los parámetros y para la vista de `startDateTime` `endDateTime` calendario.</span><span class="sxs-lookup"><span data-stu-id="58653-108">The `query` method sets the `startDateTime` and `endDateTime` parameters for the calendar view.</span></span>
    - <span data-ttu-id="58653-109">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="58653-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="58653-110">El `orderby` método ordena los resultados por hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="58653-110">The `orderby` method sorts the results by the start time.</span></span>
    - <span data-ttu-id="58653-111">El `top` método limita los resultados a 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="58653-111">The `top` method limits the results to 50 events.</span></span>

1. <span data-ttu-id="58653-112">Cree un nuevo archivo en el directorio **./routes** denominado **calendar.js** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="58653-112">Create a new file in the **./routes** directory named **calendar.js**, and add the following code.</span></span>

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const moment = require('moment-timezone');
    const iana = require('windows-iana');
    const { body, validationResult } = require('express-validator');
    const validator = require('validator');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.session.userId) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          const params = {
            active: { calendar: true }
          };

          // Get the user
          const user = req.app.locals.users[req.session.userId];
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          const timeZoneId = iana.findOneIana(user.timeZone);
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(timeZoneId.valueOf()).startOf('week').utc();
          var endOfWeek = moment(startOfWeek).add(7, 'day');
          console.log(`Start: ${startOfWeek.format()}`);

          // Get the access token
          var accessToken;
          try {
            accessToken = await getAccessToken(req.session.userId, req.app.locals.msalClient);
          } catch (err) {
            res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            return;
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              const events = await graph.getCalendarView(
                accessToken,
                startOfWeek.format(),
                endOfWeek.format(),
                user.timeZone);

              res.json(events.value);
            } catch (err) {
              res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    async function getAccessToken(userId, msalClient) {
      // Look up the user's account in the cache
      try {
        const accounts = await msalClient
          .getTokenCache()
          .getAllAccounts();

        const userAccount = accounts.find(a => a.homeAccountId === userId);

        // Get the token silently
        const response = await msalClient.acquireTokenSilent({
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI,
          account: userAccount
        });

        return response.accessToken;
      } catch (err) {
        console.log(JSON.stringify(err, Object.getOwnPropertyNames(err)));
      }
    }

    module.exports = router;
    ```

1. <span data-ttu-id="58653-113">Actualice **./app.js** para usar esta nueva ruta.</span><span class="sxs-lookup"><span data-stu-id="58653-113">Update **./app.js** to use this new route.</span></span> <span data-ttu-id="58653-114">Agregue la siguiente línea **antes de** la `var app = express();` línea.</span><span class="sxs-lookup"><span data-stu-id="58653-114">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="58653-115">Agregue la siguiente línea **después de** la `app.use('/auth', authRouter);` línea.</span><span class="sxs-lookup"><span data-stu-id="58653-115">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="58653-116">Reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="58653-116">Restart the server.</span></span> <span data-ttu-id="58653-117">Inicie sesión y haga clic en el **vínculo** Calendario de la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="58653-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="58653-118">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="58653-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="58653-119">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="58653-119">Display the results</span></span>

<span data-ttu-id="58653-120">Ahora puede agregar una vista para mostrar los resultados de una manera más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="58653-120">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="58653-121">Agregue el siguiente código en **./app.js después de** la `app.set('view engine', 'hbs');` línea.</span><span class="sxs-lookup"><span data-stu-id="58653-121">Add the following code in **./app.js after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="58653-122">Esto implementa una aplicación auxiliar [handlebars](http://handlebarsjs.com/#helpers) para dar formato a la fecha ISO 8601 devuelta por Microsoft Graph en algo más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="58653-122">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="58653-123">Cree un nuevo archivo en el directorio **./views** denominado **calendar.vhs** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="58653-123">Create a new file in the **./views** directory named **calendar.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="58653-124">Esto recorrerá una colección de eventos y agregará una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="58653-124">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="58653-125">Ahora actualice la ruta en **./routes/calendar.js** para usar esta vista.</span><span class="sxs-lookup"><span data-stu-id="58653-125">Now update the route in **./routes/calendar.js** to use this view.</span></span> <span data-ttu-id="58653-126">Reemplace la ruta existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="58653-126">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. <span data-ttu-id="58653-127">Guarde los cambios, reinicie el servidor e inicie sesión en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58653-127">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="58653-128">Haz clic en **el vínculo** Calendario y la aplicación ahora debería representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="58653-128">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
