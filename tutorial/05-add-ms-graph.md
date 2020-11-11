<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

1. Abra **./graph.js** y agregue la siguiente función dentro de `module.exports` .

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    Tenga en cuenta lo que está haciendo este código.

    - La dirección URL a la que se llamará es `/me/events` .
    - El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
    - El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.

1. Cree un nuevo archivo en el directorio **./Routes** denominado **calendar.js** y agregue el siguiente código.

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

1. Update **./app.js** para usar esta nueva ruta. Agregue la siguiente línea **antes** de la `var app = express();` línea.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Agregue la siguiente línea **después** de la `app.use('/auth', authRouter);` línea.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Reinicie el servidor. Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación. Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede Agregar una vista para mostrar los resultados de forma más fácil de uso.

1. Agregue el siguiente código en **./app.js después** de la `app.set('view engine', 'hbs');` línea.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    Esto implementa una [aplicación auxiliar de manillar](http://handlebarsjs.com/#helpers) para dar formato a la fecha ISO 8601 devuelta por Microsoft Graph en algo más fácil de ser humano.

1. Cree un nuevo archivo en el directorio **./views** denominado **Calendar. HBS** y agregue el código siguiente.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    Se recorrerá en bucle una colección de eventos y se agregará una fila de tabla para cada uno.

1. Ahora, actualice la ruta en el **/routes/calendar.js** para usar esta vista. Reemplace la ruta existente por el siguiente código.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. Guarde los cambios, reinicie el servidor e inicie sesión en la aplicación. Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
