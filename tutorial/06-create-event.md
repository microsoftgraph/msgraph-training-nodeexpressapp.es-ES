<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="968d1-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="968d1-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-a-new-event-form"></a><span data-ttu-id="968d1-102">Crear un nuevo formulario de eventos</span><span class="sxs-lookup"><span data-stu-id="968d1-102">Create a new event form</span></span>

1. <span data-ttu-id="968d1-103">Cree un archivo nuevo en el directorio **./views** denominado **newevent. HBS** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="968d1-103">Create a new file in the **./views** directory named **newevent.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/newevent.hbs" id="NewEventFormSnippet":::

1. <span data-ttu-id="968d1-104">Agregue el siguiente código al archivo **./routes/calendar.js** antes de la `module.exports = router;` línea.</span><span class="sxs-lookup"><span data-stu-id="968d1-104">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetEventFormSnippet":::

<span data-ttu-id="968d1-105">Esto implementa un formulario para la entrada del usuario y lo representa.</span><span class="sxs-lookup"><span data-stu-id="968d1-105">This implements a form for user input and renders it.</span></span>

## <a name="create-the-event"></a><span data-ttu-id="968d1-106">Crear el evento</span><span class="sxs-lookup"><span data-stu-id="968d1-106">Create the event</span></span>

1. <span data-ttu-id="968d1-107">Abra **./graph.js** y agregue la siguiente función dentro de `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="968d1-107">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="CreateEventSnippet":::

    <span data-ttu-id="968d1-108">Este código usa los campos de formulario para crear un objeto de evento de Graph y, a continuación, envía una solicitud POST al `/me/events` extremo del para crear el evento en el calendario predeterminado del usuario.</span><span class="sxs-lookup"><span data-stu-id="968d1-108">This code uses the form fields to create a Graph event object, then sends a POST request to the `/me/events` endpoint to create the event on the user's default calendar.</span></span>

1. <span data-ttu-id="968d1-109">Agregue el siguiente código al archivo **./routes/calendar.js** antes de la `module.exports = router;` línea.</span><span class="sxs-lookup"><span data-stu-id="968d1-109">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="PostEventFormSnippet":::

    <span data-ttu-id="968d1-110">Este código valida y saneado la entrada de formulario y, a continuación, llama `graph.createEvent` a para crear el evento.</span><span class="sxs-lookup"><span data-stu-id="968d1-110">This code validates and sanitized the form input, then calls `graph.createEvent` to create the event.</span></span> <span data-ttu-id="968d1-111">Se redirige de nuevo a la vista de calendario cuando finaliza la llamada.</span><span class="sxs-lookup"><span data-stu-id="968d1-111">It redirects back to the calendar view after the call completes.</span></span>

1. <span data-ttu-id="968d1-112">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="968d1-112">Save your changes and restart the app.</span></span> <span data-ttu-id="968d1-113">Haga clic en el elemento **calendario** NAV y, a continuación, haga clic en el botón **crear evento** .</span><span class="sxs-lookup"><span data-stu-id="968d1-113">Click the **Calendar** nav item, then click the **Create event** button.</span></span> <span data-ttu-id="968d1-114">Rellene los valores y haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="968d1-114">Fill in the values and click **Create**.</span></span> <span data-ttu-id="968d1-115">La aplicación vuelve a la vista de calendario una vez que se crea el nuevo evento.</span><span class="sxs-lookup"><span data-stu-id="968d1-115">The app returns to the calendar view once the new event is created.</span></span>

    ![Captura de pantalla del nuevo formulario de eventos](images/create-event-01.png)
