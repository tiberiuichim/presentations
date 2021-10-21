---
class:
  - invert
headingDivider: 2
theme: gaia
style: |
    section {
    }
---


<style>
  .hljs-name {
    color: #7ee787 !important;
  }

  .hljs-tag {
    color: #79c0ff !important;
  }

  section.invert {
    --color-background: #33507a!important;
    padding: 2em 4em !important;
  }

  section code {
    color: #c9d1d9 !important;
    background: #161b22 !important;
  }
</style>

# Volto Pluggables
<!-- _class: lead invert -->

## tl;dr

```jsx

<Pluggable name="document-top" />

//...

<Plug id="quicklinks" pluggable="document-top">
  <Button />
</Plug>

```

<!--
Now that you know (almost) everything you need to know about Pluggables, you
can decide if you wish to stick around and get into the "abstract".

Disclaimer: I didn't write the initial Pluggables library, it is a Volto port
of react-slot-fill.
-->

## Who am I?

- Tiberiu Ichim
- Zope and Plone developer since 2003
- Eaudeweb

<!--  4 minutes

For those of you who don't know me, my name is Tiberiu Ichim, I'm a Plone/Volto
developer working with Eaudeweb Romania. I'm a core Volto contributor and I've
been developing websites with Volto for about 2 years.

Our main client for the Volto websites is EEA, the European Environmental Agency.
Through our work, they are a big contributor to the Volto ecosystem and they're
one of the so-called early adopters of Volto.

I think it's important, as an anecdote, to understand the impact of Volto at
EEA. In the autumn of 2019, at the dawn of my/our Volto history,
I was giving a presentation to the EEA technical staff on our advantages of
adopting Volto. It was a dry and very technical experience because I've wanted
to put the "bad cards" on the table first, before showing something that I knew
would get them excited: an integration with the Plotly Chart Editor, structured
as a sort of primordial addon. And that was running on top of "old trusty Plone".
Needless to say, they were immediately excited and saw the immense advantages
Volto could bring to the overall architecture.

As a consequence, most of the public EEA Plone presence is now already on Volto
or in the process of being migrated to Volto.

-->

## Why do we need Pluggables?
<!-- _class: lead invert -->

<!--
I see Pluggables as a way to provide scalability to Volto interactions and I'll
walk you through this train of thought.
-->

## Scaling Volto

- Addons!

<!--
One of our first concerns was: how do we scale Volto? We knew our work
landscape: multiple websites, small teams, so the "addons story" was one of the
big first contributions that we made to the Volto project.
-->

## EEA Addons

TODO: include eea-volto-gh-search.png

<!--

Since then EEA has published over 80 open source Volto addons, websites, Plone
integration addons, etc. All open in the EEA github organisations. So if you're
looking for real examples on how to achieve something with Volto, there's
plenty of examples. And of course there are many companies with open source
code: RedTurtles, CodeSyntax, Rohberg and of course KitConcept. See the Volto
readme page for this.

-->

## Addon to an addon

- how do you express an "add-on to an add-on"?
- we need Volto's equivalent of ZCA

<!--
So far we've scaled Volto with addons. But we're already starting to see that
some addons need to provide extension mechanisms. volto-slate has 3 or 4 addons
to the addon. We're always finding new ways to abuse UX with the columns block
or tabs block, etc.

So we need a deeply integrated extensibility, just like Plone has with ZCA.

One of the things that make Volto really attractive is developer friendlines.
I've seen this many many times already, new developers can become productive
very fast with Volto. So we have to keep things light and understandable and
don't scare them with dependency injection or component lookup in an opaque
registry.
-->

## React data flow

- In React, data flow is top to bottom
- "out of tree" data needs Redux
- how to interact with foreign components?

<!--
- A generic framework to enable pluggability and configuration "from the
  outside".

In React world the "top-bottom" approach is strict. Components pass properties
to their children, children can call functions passed down as props. To enable
communication between arbitrary component trees you need Redux (or something
equivalent). Why? Components need to "update" when state outside them changes.

This makes the components "frozen" in their implementation. We can make them
configurable and extendible, but we need to explicitely program and design this
extensibility for each one of them. One example of an explicit extensibility
mechanism is Volto's "block variations", where you have to write to a central
registry.

When programming Plone the ZCA is its most basic language. Everything is
a component, writing an interface and an adaptor is the most natural thing.
Because of this, we're pretty much guaranteed pluggability almost everywhere.
-->

## UI is not static

The UI state is fluid

<!--
The state will always change

TODO: idea: it's not easy to model transient things as configuration
-->

## Do we need pluggability?

- Pluggable = reusable, scalable
- Pluggability is hard. Design upfront.
- Volto blocks are extensible through dedicated API
- How deep to go with declared configuration?

<!--
- Components can now declare "slots"
- Replaces `<Portal>`
-

Pluggability enables scalability and reuse. Because of "programmed" (via the
configuration) pluggability, Volto blocks can be recycled: a new view template
can reuse the block data to show things in a different way, the variations can
extend the block schema model to add new features to the block.

But pluggability is also hard: it requires designing an API and a pluggability
model. With ZCA this is reduced as there are already established patterns and
best practices.
-->

## Pluggables ~= interactive viewlets

<!--
Volto's pluggability needs are usually visual but also based on interactivity
(it's an UI, after all).

A good example of pluggable UI in Plone is the viewlet manager. You declare it
once, you include it in the template and it will render things inside it.

TODO: add ex of ZCML code paralel with Pluggable
-->

## `<Pluggable>` = `<browser:viewletManager>`

```
<browser:viewletManager name="aboveDocumentTitle" />
```
vs

```
<Pluggable name="aboveDocumentTitle" />
```

## `<Plug>` = `<browser:viewlet>`

```
<browser:viewlet manager="" />
```
vs
```
<Plug pluggable="aboveDocumentTitle" id="title" order={100} />
```

<!--
But Plone's "pluggables" are quite static. You can trace the request-publish
cycle all the way to the CGI and the beginning of web apps. Request, response,
rinse, repeat.

With Volto being a SPA, the whole application state is continously shifting and
mutating. The interactivity in Volto needs to go beyond "display this
additional thing here". It even needs to allow inter-component communication,
passing down props "out of tree" and more, as a generic framework.
-->

## Advanced patterns

- Overwrite a <Plug> with a <Plug>

```
<Plug id="delete-button" pluggable="toolbar"><Button color="blue"></Plug>
```

Later, render a Plug with the same id

```
<Plug id="delete-button" pluggable="toolbar"><Button color="red"></Plug>
```

<!--
- Plugs can override other plugs. Watch out for the "dependencies" prop to make
  sure you keep the best one "alive" `<Plug dependencies={[...]}>`
-->

## Advanced patterns (2)

- Read data from the <Pluggable>

<!--
- Plugs can read data from the Pluggable. `<Pluggable params={...}>`
-->

## Interact with the pluggable!

- Pass down a function from the <Pluggable>!

## Use cases

- Toolbars
- The Quanta Toolbar

<!--
- Toolbars hit all the marks for Pluggables: they're highly interactive,
  dependent on context

- The Quanta toolbar: the convention is that each block has a single toolbar.
  But what if we want to add things to that toolbar, things that make sense in
  other non-standard use case, for example reuse the blocks as slots and add
  slot-specific buttons.

- What if an addon wants to add a button? For example, right now
  volto-block-style needs to add a button to the right-top-side, as there's no
  pluggable way to add controls to blocks. A guaranteed pluggable toolbar for
  blocks would enable that.
-->

## Showcase
https://github.com/plone/volto/blob/2d8f943a8c82795b2068b58a2a7c07c56fd41d80/src/components/manage/Blocks/Block/GroupedMenuButtons.jsx

## Limitations

- No SSR
- Watch for dependency lists!
- Limited adoption (yet)

<!--
Pluggables are not SSR-enabled. Is this a problem? Not really. Slots are an
alternative and the plan is to make slots pluggables-enabled.
-->

## Implementation (simplified)

The context

```jsx
<PluggablesProvider>
  <Pluggable name="top" />
  <Plug id="delete" />
</PluggablesProvider>
```

<!--
The key to understanding Pluggables is in understanding the implementation.

<PluggablesProvider> provides a context and a state. As use effect, plugs write
their children into that global context. Pluggables subscribe to that global
state and render the "Plug renderers".

The implementation is a Volto port of ...
-->

## Implementation (2)

The Plug

```jsx
const Plug = ({id, children}) => {
  const { register } = useContext(PluggablesProvider.Context);

  React.useEffect(() => {
    register(id, () => children);
  });

  return null;
}
```

## Implementation (3)

The Pluggable

```jsx
const Pluggable = (name) => {
  const { getPlugs } => useContext(PluggablesProvider.Context);
  return getPlugs(name).map(f => f());
}
```

## Thank you!
<!-- _class: lead invert -->
