<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="101af-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="101af-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="101af-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="101af-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="101af-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="101af-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="101af-104">Empiece agregando un nuevo método al `./graph.js` archivo para obtener los eventos del calendario.</span><span class="sxs-lookup"><span data-stu-id="101af-104">Start by adding a new method to the `./graph.js` file to get the events from the calendar.</span></span> <span data-ttu-id="101af-105">Agregue la siguiente función dentro de `module.exports` in `./graph.js`.</span><span class="sxs-lookup"><span data-stu-id="101af-105">Add the following function inside the `module.exports` in `./graph.js`.</span></span>

```js
getEvents: async function(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

<span data-ttu-id="101af-106">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="101af-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="101af-107">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="101af-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="101af-108">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="101af-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="101af-109">El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="101af-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="101af-110">Cree un nuevo archivo en el `./routes` directorio denominado `calendar.js`y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="101af-110">Create a new file in the `./routes` directory named `calendar.js`, and add the following code.</span></span>

```js
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
    }
  }
);

module.exports = router;
```

<span data-ttu-id="101af-111">Actualice `./app.js` para usar esta nueva ruta.</span><span class="sxs-lookup"><span data-stu-id="101af-111">Update `./app.js` to use this new route.</span></span> <span data-ttu-id="101af-112">Agregue la siguiente línea **antes** de `var app = express();` la línea.</span><span class="sxs-lookup"><span data-stu-id="101af-112">Add the following line **before** the `var app = express();` line.</span></span>

```js
var calendarRouter = require('./routes/calendar');
```

<span data-ttu-id="101af-113">A continuación, agregue la \*\*\*\* siguiente línea `app.use('/auth', authRouter);` después de la línea.</span><span class="sxs-lookup"><span data-stu-id="101af-113">Then add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

```js
app.use('/calendar', calendarRouter);
```

<span data-ttu-id="101af-114">Ahora puede probar esto.</span><span class="sxs-lookup"><span data-stu-id="101af-114">Now you can test this.</span></span> <span data-ttu-id="101af-115">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="101af-115">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="101af-116">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="101af-116">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="101af-117">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="101af-117">Display the results</span></span>

<span data-ttu-id="101af-118">Ahora puede Agregar una vista para mostrar los resultados de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="101af-118">Now you can add a view to display the results in a more user-friendly manner.</span></span> <span data-ttu-id="101af-119">En primer lugar, agregue el siguiente `./app.js` código en `app.set('view engine', 'hbs');` **después** de la línea.</span><span class="sxs-lookup"><span data-stu-id="101af-119">First, add the following code in `./app.js` **after** the `app.set('view engine', 'hbs');` line.</span></span>

```js
var hbs = require('hbs');
var moment = require('moment');
// Helper to format date/time sent by Graph
hbs.registerHelper('eventDateTime', function(dateTime){
  return moment(dateTime).format('M/D/YY h:mm A');
});
```

<span data-ttu-id="101af-120">Esto implementa una [aplicación auxiliar](http://handlebarsjs.com/#helpers) de manillar para dar formato a la fecha ISO 8601 devuelta por Microsoft Graph en algo más fácil de ser humano.</span><span class="sxs-lookup"><span data-stu-id="101af-120">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

<span data-ttu-id="101af-121">Cree un nuevo archivo en el `./views` directorio denominado `calendar.hbs` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="101af-121">Create a new file in the `./views` directory named `calendar.hbs` and add the following code.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    {{#each events}}
      <tr>
        <td>{{this.organizer.emailAddress.name}}</td>
        <td>{{this.subject}}</td>
        <td>{{eventDateTime this.start.dateTime}}</td>
        <td>{{eventDateTime this.end.dateTime}}</td>
      </tr>
    {{/each}}
  </tbody>
</table>
```

<span data-ttu-id="101af-122">Se recorrerá en bucle una colección de eventos y se agregará una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="101af-122">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="101af-123">Ahora, actualice la ruta `./routes/calendar.js` en para usar esta vista.</span><span class="sxs-lookup"><span data-stu-id="101af-123">Now update the route in `./routes/calendar.js` to use this view.</span></span> <span data-ttu-id="101af-124">Reemplace la ruta existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="101af-124">Replace the existing route with the following code.</span></span>

```js
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
        req.flash('error_msg', {
          message: 'Could not get access token. Try signing out and signing in again.',
          debug: JSON.stringify(err)
        });
      }

      if (accessToken && accessToken.length > 0) {
        try {
          // Get the events
          var events = await graph.getEvents(accessToken);
          params.events = events.value;
        } catch (err) {
          req.flash('error_msg', {
            message: 'Could not fetch events',
            debug: JSON.stringify(err)
          });
        }
      }

      res.render('calendar', params);
    }
  }
);
```

<span data-ttu-id="101af-125">Guarde los cambios, reinicie el servidor e inicie sesión en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="101af-125">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="101af-126">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="101af-126">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
