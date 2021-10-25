# Volto Slots

##  Problem: Volto has no viewlet-equivalent

Solution:

```
import { ContextNavigation } from '@plone/volto/components';

config.slots.asideRightSlot: {
    title: 'Right column',
    manage: false,
    items: [{
         id: 'ContextNavigation',
         component: ContextNavigation,
     }]
   }
```

## Problem: Volto has no portlets

Solution:

```
config.slots.asideRightSlot: {
    title: 'Right column',
    manage: true,
    items: []
  }}
```

## Idea: reuse Volto blocks as portlets

<!--
Reuse the Volto blocks as an engine for "layout things".
-->

## Let's rewind

* Volto has no portlets
* Volto wants portlets
* Volto embraces newbies & frontend developers

<!--
Context:

Volto has no portlets. If you dig hard enough, you'll find a plone.restapi PR
with portlet serialization and there's even a Volto portlet renderer that I've
implemented as part of FISE, but the "classic" Plone portlets are not
officially supported by Volto.

The portlets themselves are a great concept. Not every project needs them, but
it's good to have them in the "arsenal".

With Volto we're trying to empower the frontend developers, so people with no
knowledge of Plone can make a significant contributions to Plone
projects. With Volto it is actually possible for someone with almost 0 Plone
"backend" knowledge to create and develop a real project.
-->

## How can we improve portlets?

* Simplify configuration.
* Volto blocks are very expressive. We can fill the gaps
* "Modify portal content" for slots
* UI power = more capabilities:
  - atomic blocking of parent blocks
  - override parent blocks
  - remix parent with local blocks

<!--
What can we do to improve the portlet story? These are the propositions of the
slots:

- Simplify configuration of slots. That's just 2 lines of JSON configuration in
  Volto
- reuse the existing Volto blocks as portlets
- Cover missing gaps with new dedicated Volto blocks, such as navigation
- Allow selectively managing slots as content, editable with the `Modify
  portal content` permission.
- Having Volto and React power, it's easier to create UI that expresses more
  complex scenarios:
    - atomic blocking of inherited blocks, instead of all or nothing
    - create local copies of blocks
    - mix order of "local" and inherited blocks
-->

## How can we use them?

- Sidebars: listings, info boxes, navigation, etc
- section headers, content
- site chrome: fat menus, footers
- as a generic registry?

<!--
What can we do with them?

- sidebars: info boxes, navigation, etc
- "fat menus".
- section headers, content
- footers
- etc
- Wild scenarios, such as using the slots endpoint as a registry for site-wide
  configuration. A "TTW block designer" could use the slots to store its
  presets.
-->

## Current status

- plone.restapi PR, > 100 commits
  - basic functionality around 80% ready
- Volto PR, > 260 commits
  - basic functionality around 80% ready

## Already used live!

## Live demo

## The Quanta Toolbar
