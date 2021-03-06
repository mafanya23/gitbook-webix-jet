# App Configuration

An app module is created as a new instance of the JetApp class. You must pass an object with your app configuration to the app constructor. You can include various parameters into the app configuration.

| Parameter | What For |
|-----------|----------|
| [start](#start)     | to set the start app URL|
| [debug](#debug)     | to enable debugging |
| [router](#router)   | to change a router |
| [arbitrary parameters](#other) | e.g. access mode, screen size, etc |
| [views](#views)     | to change view modules names |
| [routes](#routes)   | to shorten the app URL |

### <span id="start">Start URL</span>

The app UI will be rendered from the URL elements from **start** when you open the app for the first time. The app splits the URL into parts, finds the corresponding files in the **views** folder and creates an interface by combining UI modules from those files.

~~~js
// myapp.js
import {JetApp} from "webix-jet";

const app = new JetApp({
    start:"/top/layout"
}).render();
~~~

### <span id="debug">Debugging</span>

You can enable debugging in the app configuration: 

```js
// myapp.js
import {JetApp} from "webix-jet";

const app = new JetApp({
    start: "/top/about",
    debug: true
}).render();
```

With *debug:true*, error messages will be logged into console and a debugger will pause the app on errors.

### <span id="other">Various App Parameters</span>

In the app config, for example, you can set the mode in which the app will work:

```js
// myapp.js
import {JetApp} from "webix-jet";

const app = new JetApp({
	mode:"readonly",  //application wide configuration
	start:"/top/layout"
}).render();
```

Later in the code, you can do some actions according to the mode:

```js
// views/games.js
...
if (this.app.config.mode === "readonly"){
	this.show("limited");
}
...
```

**this.app** refers to the app, while **this** refers to the view<sup><a href="#myfootnote1" id="origin">1</a></sup>.

### <span id="router">Routers</span>

New Webix Jet has four types of routers. You should specify the preferred router in the app configuration as well. The default router is *HashRouter*. If you don't want to display the hashbang in the URL, you can change the router to *UrlRouter*:

```js
// myapp.js
import {UrlRouter,JetApp} from "webix-jet";

const app = new JetApp({
	router: UrlRouter,
    start:"/top/layout"
}).render();
```

### <span id="views">Changing View Names</span>

If the module you want to show is in a subfolder and you want to show the module with a shorter name, you can change the view name in app configuration:

```js
// myapp.js
import {JetApp} from "webix-jet";

const app = new JetApp({
    start: "/top/start",
    views: {
        "start" : "area.list" // load /views/area/list.js
    }
}).render();
```

In this example, **list** module is stored in the **area** subfolder in */views* (/views/area/list.js). Later, you can show the view by the new name, e.g.:

```js
// views/top
import {JetView} from "webix-jet";

export default class TopView extends JetView {
	config(){
		return {
			cols:[
                { view:"button", value:"start",
                  click:(id) => {
					this.show("start");
				}},
				{ $subview: true }
			]
		};
	}
}
```

**this** in the button handler refers to the Jet view, because the handler is an arrow function<sup><a href="#myfootnote1" id="origin1">1</a></sup>.

[Check out the demo >>](https://github.com/webix-hub/jet-demos/blob/master/sources/viewresolve.js)

### <span id="routes">Beautifying the URL</span>

If you do not want to display some part of the app URL, you can hide it with the help of **routes** in app configuration. For instance, you might want to display only the names of subviews in the URL: 

```js
// myapp.js
import {JetView} from "webix-jet";

const app = new JetApp({
    start: "/top/about",
    routes: {
        "/hi" 	: "/top/about",
        "/form" : "/top/area.left.form",
        "/list" : "/top/area.list",
    }
});
```

Instead of a long URL with subdirectories, e.g. *"/top/area.left.form"*, the app URL will be displayed as */form*.

[Check out the demo >>](https://github.com/webix-hub/jet-demos/blob/master/sources/routes.js)

<!-- footnotes -->
- - -
<a id="myfootnote1" href="#origin1">1 &uarr;</a>:
To read more about how to reference apps and view classes, go to ["Referencing views"](../detailed/referencing.md).