# HTML

It is pretty easy to embed an Elm program in an HTML node. This is a great way to start introducing Elm gradually into an existing project.

## Embedding Elm in HTML

Normally when you run the Elm compiler, it will give you an HTML file that sets everything up for you. So running this:

```bash
elm make src/MyThing.elm
```

Will result in a `index.html` file that you can just open up and start using. To do fancier stuff, we want to compile to JavaScript, so we modify the command slightly:

```bash
elm make src/MyThing.elm --output=my-thing.js
```

We specify that the output file is a JS file. The compiler will generate a JavaScript file that sets up the following stuff:

```javascript
var Elm = {};
Elm.MyThing = {
    fullscreen: function() { /* take over the <body> */ },
    embed: function(node) { /* take over the given node */ },
    worker: function() { /* run the program with no UI */ }
};
```

So to actually embed your Elm program in an HTML node, you will want to add something like this to your HTML:

```html
<div id="my-thing"></div>
<script src="my-thing.js"></script>
<script>
    var node = document.getElementById('my-thing');
    var app = Elm.MyThing.embed(node);
</script>
```

This is doing three important things:

  1. We create a `<div>` that will hold the Elm program.
  2. We load the JavaScript generated by the Elm compiler.
  3. We grab the relevant node and initialize our Elm program in it.

So now we can set Elm up in any `<div>` we want. So if you are using React, you can create a component that just sets this kind of thing up. If you are using Angular or Ember or something else, it should not be too crazy either. Just take over a `<div>`.

The next section gets into how to get your Elm and JavaScript code to communicate with each other in a nice way.