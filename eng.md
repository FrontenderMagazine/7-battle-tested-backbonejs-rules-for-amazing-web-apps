We've been using [Backbone.js][1] at [Bizzabo][2] for the past 2 years. We
started out with a small web app and we now have several apps powered by 
Backbone and its[close][3] [friends][4].

**Backbone is unopinionated by design.** The basic idea you get from the
documentation is: do whatever you feel with the tools backbone.js provides.

This is great because it fits so many different use cases and it's so easy to
start writing apps. This approach also led us to make a few rookie mistakes when
we first started out.

**What we did get right was detecting when something was wrong and finding the
cure for it.**

Instead of repeating our own mistakes, follow our rules and tips for manageable
Backbone.js development:

## 1. Views are Data-Less

Data belongs to models not views. Next time you find yourself storing data
inside a view (or worse: the DOM), stop and move it into the model.

If you don’t have a model, create one, it’s easy:

It really doesn’t have to be any fancier than that.

You can now listen to change events on all your data or even easily sync it
with your server and create a real time experience.

## 2. DOM Events only affect models

When a DOM event occurs, such as a click on a button, don’t let it change the
view itself. Change the model.

Changing the DOM without changing a state means you keep your state on the DOM
. This rule will keep you from having an inconsistent state.

If a click on a “read more” link was made, don’t expand the view, just
change the model:

Ok, but how will the view actually change? Good question, the next rule answers
that.

## 3. DOM Changes only when the model changes

Events are awesome - use them. The easiest approach is to render again after
each change:

A much better approach is to only change what’s needed:

The view is always synced with the model. It doesn’t matter how the model was
changed: following an action, from a fetch or from a console debugging session
- the view will always stay up to date.

## 4. What is bound must be unbound

When a view is removed from the DOM using the \`remove\` method, it must unbind
from all the events it is bound to.

If you've used \`on\` for binding, it's your duty to use \`off\` for unbinding
. Without unbinding, the garbage collector can't release the memory for you and 
your apps performance will degrade.

That's where \`listenTo\` comes in. It tracks what the view is bound to and
unbinds it on a remove. Backbone does this by calling \`stopListening\` just 
before removing itself from the DOM:

## 5. Always be chainable

Always return \`this\` from the \`render\` and \`remove\` functions. This
allows you to chain actions:

This is the convention, do not break it.

## 6. Events are better than callbacks

It's better to react upon events than waiting for callbacks.

Backbone models fire 'sync' and 'error' events by default so we can use these
events instead of providing callbacks. Consider these two alternatives:

This is much better:

It doesn't matter how or where a model will be fetched, the handleSuccess/
handleError methods will be called.

## 7. Views are scoped

A view should never(!) operate on a DOM other than its own.

The view has a reference to its own DOM element at 'el' and also a jQuery
element '$el
'.

This means you must never use jQuery directly:

Always scope yourself to your own DOM element instead:

If you need to alter a different view, just trigger an event and let the other
view do its thing. You can also use Backbone as a global Pub/Sub system.

For example, let's prevent the page from scrolling:

## One more thing

You can learn a lot by just going over backbone's code. Check out the 
[backbone.js annotated source][5] and discover how the "magic" happens. The
library is small and reading the entire code shouldn't take more than 10 minutes.
 

These tips help us write cleaner, better and easier-to-read code.

## Love writing great code?

We're looking for passionate developers to [join us][6] at our Tel Aviv office
.

 [1]: http://backbonejs.org/
 [2]: http://www.bizzabo.com
 [3]: http://requirejs.org
 [4]: http://handlebarsjs.com/
 [5]: http://backbonejs.org/docs/backbone.html
 [6]: http://www.bizzabo.com/jobs#frontend