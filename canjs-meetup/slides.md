title: DoneJS - Get your app done
output: index.html
theme: theme
controls: false
logo: theme/logo.png

--

# DoneJS - Get your app done

-- presenter

![Matthew Phillips](https://avatars2.githubusercontent.com/u/361671?v=3&s=200)

## Matthew Phillips

* [<i class="fa fa-github"></i> matthewp](https://github.com/matthewp)
* [<i class="fa fa-twitter"></i> @matthewcp](http://twitter.com/matthewcp)

-- presenter

![David Luecke](http://gravatar.com/avatar/a14850281f19396480bdba4aab2d52ef?s=200)

## David Luecke

* [<i class="fa fa-github"></i> daffl](https://github.com/daffl)
* [<i class="fa fa-twitter"></i> @daffl](http://twitter.com/daffl)

--

# Components

--

## Benefits

* Declaratively write your application.
* Separation between View Model and View logic.

## Warts

* Boilerplate.

--

## done-component

A StealJS plugin that allows composing CanJS Components in a single file:

### hello.component

```
<can-component tag="hello-greeting">
  <template>
    <h1>Hello {{name}}!</h1>
  </template>
  <view-model>
    import Map from "can/map/";

    export default Map.extend({
      name: ""
    });
  </view-model>
</can-component>
```

--

### Output

```
<hello-greeting name="world">

  <h1>Hello world!</h1>

<hello-greeting>
```

--

# Server Side Rendering

Getting rid of the loading indicator.

--

## Benefits

* SEO
* Perceived Performance

--

## Implementation

### Single Context Virtual DOM

* Everything runs in a single Node.js context.
* Uses a very lightweight DOM.
* Everything is connected to the render lifecycle.

--

### Connected Lifecycle

```
curl http://place-my-order.com/restaurants/cow-barn/order
```

```
<html>
<head>
  <link rel="stylesheet" href="/dist/bundles/pmo/index.css">
  <link rel="stylesheet" href="/dist/bundles/pmo/order/new/new.css">
</head>
<body>
  ...

  <script>
    INLINE_CACHE = {"restaurant":{"{..."}}}
  </script>
</body>
</html>
```

--

### lib/app.js

```
var app = require("express")();
var url = require("url");

var render = require("can-ssr")({
  config: __dirname + "/package.json!npm"
});

app.get("/"), function(req, res){

  var pathname = url.parse(req.url).pathname;

  render(pathname).then(function(html){
    res.send(html);
  });

});

app.listen(7000);
```

--

### pmo/restaurant/list.js

```
export const ViewModel = Map.extend({
  states: { get() { return State.findAll({}); } }, ...});
  
export default Component.extend({
  tag: 'app-restaurant-list',
  viewModel: ViewModel,
  template,
  events: {
    inserted: function(){
      var statesPromise = this.attr("states");
      this.scope.attr("@root").pageData( statesPromise );
    }
  }
});
```

--

## Server Side Rendering Techniques

| Technique         | Maintainability    | Speed  |
| ----------------- |:------------------:| ------:|
| Duplicate Code    | 0                  | 10     |
| Headless Browser  | 9                  | 5      |
| Virtual DOM       | 8                  | 8      |

--

# Production Workflows

--

## Build for any Environment

* The Web
* iOS and Android (Cordova)
* Desktop (NW.js)

--

## Steal Tools
