
# Events

This section will help you understand how to register your own callbacks to events in the router.
It will also cover the basics of event-handlers; how to use the handlers provided with the router and how to create your own custom event-handlers.

## Available events

This section contains all available events that can be registered using the `EventHandler`.

All event callbacks will retrieve a `EventArgument` object as parameter. This object contains easy access to event-name, router- and request instance and any special event-arguments related to the given event. You can see what special event arguments each event returns in the list below.  

| Name                        | Special arguments | Description               | 
| -------------               |-----------      | ----                      | 
| `EVENT_ALL`                 | - | Fires when a event is triggered. |
| `EVENT_INIT`                | - | Fires when router is initializing and before routes are loaded. |
| `EVENT_LOAD`                | `loadedRoutes` | Fires when all routes has been loaded and rendered, just before the output is returned. |
| `EVENT_ADD_ROUTE`           | `route` | Fires when route is added to the router. |
| `EVENT_REWRITE`             | `rewriteUrl`<br>`rewriteRoute` | Fires when a url-rewrite is and just before the routes are re-initialized. |
| `EVENT_BOOT`                | `bootmanagers` | Fires when the router is booting. This happens just before boot-managers are rendered and before any routes has been loaded. |
| `EVENT_RENDER_BOOTMANAGER`  | `bootmanagers`<br>`bootmanager` | Fires before a boot-manager is rendered. |
| `EVENT_LOAD_ROUTES`         | `routes` | Fires when the router is about to load all routes. |
| `EVENT_FIND_ROUTE`          | `name` | Fires whenever the `findRoute` method is called within the `Router`. This usually happens when the router tries to find routes that contains a certain url, usually after the `EventHandler::EVENT_GET_URL` event. |
| `EVENT_GET_URL`             | `name`<br>`parameters`<br>`getParams` | Fires whenever the `Router::getUrl` method or `url`-helper function is called and the router tries to find the route. |
| `EVENT_MATCH_ROUTE`         | `route` | Fires when a route is matched and valid (correct request-type etc). and before the route is rendered. |
| `EVENT_RENDER_ROUTE`        | `route` | Fires before a route is rendered. |
| `EVENT_LOAD_EXCEPTIONS`     | `exception`<br>`exceptionHandlers` | Fires when the router is loading exception-handlers. |
| `EVENT_RENDER_EXCEPTION`    | `exception`<br>`exceptionHandler`<br>`exceptionHandlers` | Fires before the router is rendering a exception-handler. |
| `EVENT_RENDER_MIDDLEWARES`  | `route`<br>`middlewares` | Fires before middlewares for a route is rendered. |
| `EVENT_RENDER_CSRF`         | `csrfVerifier` | Fires before the CSRF-verifier is rendered. |

## Registering new event

To register a new event you need to create a new instance of the `EventHandler` object. On this object you can add as many callbacks as you like by calling the `registerEvent` method.

When you've registered events, make sure to add it to the router by calling 
`Course::addEventHandler()`. We recommend that you add your event-handlers within your `routes.php`.

**Example:**

```php
use Solital\Core\Course\Handlers\EventHandler;
use Solital\Core\Course\Event\EventArgument;

// --- your routes goes here ---

$eventHandler = new EventHandler();

// Add event that fires when a route is rendered
$eventHandler->register(EventHandler::EVENT_RENDER_ROUTE, function(EventArgument $argument) {
   
   // Get the route by using the special argument for this event.
   $route = $argument->route;
   
   // DO STUFF...
    
});

Course::addEventHandler($eventHandler);

```

## Custom EventHandlers

`EventHandler` is the class that manages events and must inherit from the `EventHandlerInterface` interface. The handler knows how to handle events for the given handler-type. 

Most of the time the basic `\Solital\Core\Course\Handler\EventHandler` class will be more than enough for most people as you simply register an event which fires when triggered.

Let's go over how to create your very own event-handler class.

Below is a basic example of a custom event-handler called `DatabaseDebugHandler`. The idea of the sample below is to logs all events to the database when triggered. Hopefully it will be enough to give you an idea on how the event-handlers work.

```php
namespace Demo\Handlers;

use Solital\Core\Course\Event\EventArgument;
use Solital\Core\Course\Router;

class DatabaseDebugHandler implements EventHandlerInterface
{

    /**
     * Debug callback
     * @var \Closure
     */
    protected $callback;

    public function __construct()
    {
        $this->callback = function (EventArgument $argument) {
            // todo: store log in database
        };
    }

    /**
     * Get events.
     *
     * @param string|null $name Filter events by name.
     * @return array
     */
    public function getEvents(?string $name): array
    {
        return [
            $name => [
                $this->callback,
            ],
        ];
    }

    /**
     * Fires any events registered with given event-name
     *
     * @param Router $router Router instance
     * @param string $name Event name
     * @param array ...$eventArgs Event arguments
     */
    public function fireEvents(Router $router, string $name, ...$eventArgs): void
    {
        $callback = $this->callback;
        $callback(new EventArgument($router, $eventArgs));
    }

    /**
     * Set debug callback
     *
     * @param \Closure $event
     */
    public function setCallback(\Closure $event): void
    {
        $this->callback = $event;
    }

}
```
---