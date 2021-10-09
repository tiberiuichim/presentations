# volto-slate

I'm the original lead developer for volto-slate, it was developed as part of
the BISE project for EEA.

Probably the most important Volto addon, right now. If you start a new Volto
project, it is one addon that you should include in your project.

Nobody wants to think about the editor. We mostly take them for granted. But
volto-slate is probably not even the fifth in line of Plone richtext editors
(Kupu, FCK, Tiny, Draft, etc).

Requirements change, the web changes, etc. And if I'd have time today, I might
even start another volto-slate alternative.

### volto slate demo

It's boring, just like a text editor should be.

- Show off the style menu
- Show off editing data entities
- Show off editing footnotes, zotero

## History

- in winter 2019-2020 we were working on forest.eea.europa.eu and the mockup
  included text that intermixed description with numbers coming from the
  database. We were also in a position where it wasn't clear, from the business
  point of view, what the final output would be, so I've had an idea to create
  a framework for "live documents", to make Volto as flexible as possible, so
  when the day comes, producing the final output will not be my job, as
  a programmer, but that of the site editors.

  I've started working on new draftjs plugins, but I was having problems
  (components were not refreshing, draftjs API was bad) not a positive one
  experience. In reality I was trying to push it even beyond what was
  actually needed for the job.

  I was also hitting problems with the final output rendering: I didn't have to
  deal only with draftjs, but also differences and bugs in redraft, another
  library that can take draftjs trees and render them.

  Around spring 2020 we've started to draft the addon infrastructure, so I had
  the idea of creating an addon from scratch, to understand the needs of that
  addon infrastructure. As I've already wanted to test-drive slate, I've
  started to make a "toy addon", the volto-slate.

## volto-slate ecosystem

The volto-slate has a number of its own plugins:

- volto-slate-dataentity
- volto-slate-footnote
- volto-slate-metadata-mentions
- volto-slate-zotero

## Rough comparison draftjs vs slate

- Slatejs backed by an open source community, just like Plone
- draftjs is from Facebook

SlateJS has a lot more OS activity and there's even contributions from TinyMCE
developers.

https://github.com/ianstormtaylor/slate/pull/4518

## What is SlateJS

The key to understanding exactly what volto-slate means is understanding the
underlying library, SlateJS. SlateJS is not an editor, it is a framework to
build rich text editor. What types of elements, how they are rendered, etc,
nothing like that is defined in any of the slatejs packages.

With volto-slate we're trying to build a superset of an HTML-compatible
richtext editor. Why HTML compatible? Even if it's complex and we'd rather deal
with semantically marked up elements, being so ubiquitous,
interoperability will never be a problem. You will find libraries and tools to
handle it, and external systems (indexing, etc) are usually equiped to deal
with it.

## A look at slate internal trees

Slate JSON

Unlike other modern editors that use the concept of "marks" for decorative
tags (strong, em, etc), volto-slate always uses hard Element nodes. Initially
we've gone that route as well, but switched when we've realized that when
ingesting HTML, there's no way to render identical markup similar to the
source. Example (pseudocode):

```
{type: 'a', marks: ['b']}
```

Does it render as `<b><a /></b>` or `<a><b></b></a>`. Does it matter? For
styling purposes and accuracy, yes, it does.

## Slate output rendering

Being close to the HTML DOM model, the rendering is a single recursive function
that just outputs React components and their children. (pseudocode)

```
const serializeNodes(nodes) =>
  nodes.map(node => isText(node)
    ? <Leaf>{node.text}<Leaf>
    : <Element mode='view'>{serializeNodes(node.children)}</Element>);
```

This is just "converting", the actual rendering of the component is done by
React. To convert to real HTML, in the HTML-saving widget, we also use React.

## Deep understanding of Volto

With volto-slate we've tried to anticipate the editor workflow, as they're used
to modern web editors. The quality of those web apps is extremely high and so
those expections are reflected also to interactions with Volto. The Volto
blocks presented a challenge, but the basic rule is obvious: one paragraph per
block, Volto provides separate blocks for things like images and tables.

So we have rules such as:

- hit enter in the middle of a block it will split the block in two
- unless you're in a list
- backspace at the beginning of a line joins it with the previous block
- ...

And because the basic slate block becomes the default UI for interaction,
we're also providing additional functionality:

- when pasting HTML, it converts it to Volto blocks: multiple paragraphs as
  multiple Volto blocks, images transformed to Volto image blocks, tables to
  Volto table blocks and so on.
- you can copy/paste a google spreadsheet and we'll convert it to a table
- you can drag/drop an image in a slate block and we'll create a new Volto
  image block.

## A layered architecutre

The component hierarchy

At the root sits the SlateEditor component, it is the most basic one. On top of
it are built the block-level components (TextBlock with its DetachedTextBlock
variant), then RichTextWidget and HTMLWidget.

Despite all those Volto-blocks specific features, the TextBlock uses only
a couple of extra plugins to implement that functionality (mostly related to
keyboard) and a single principle: at the end of some operations (such as
paste), we call a single function, `deconstructToVoltoBlocks`, which uses
configurable `voltoBlockEmiters` to convert Slate nodes to Volto blocks and
then splits the multiple paragraphs to separate blocks.

## The text block

It works in two modes "detached" and "one paragraph per block". The detached
mode is a text block with multi-paragraph mode enabled. It is used, for
example, by `@kitconcept/volto-blocks-grid` which provides a single dimensional
grid.

## Configuration of text block

Almost all of the behaviour of the text block (and that of the basic text
editor, where is the case) comes from configuration. There's multiple levels of
configuration:

- the basic text editor
- the text editor block
- keyboard handlers, shortcuts, behaviour, everything is exposed as
  configuration and can be tweaked from your Volto project.

A lot of this customizability and progressive enhancement is also thanks to the
slate editor, which uses an extremely simple and effective idea to provide
a plugin system:

```
const withInlineLink = (editor) => {
  const originalIsInline = editor.isInline;
  editor.isInline = (node) => {
    return (node.type === 'link') ? true : originalIsInline(node);
  }
}
editor = withInlineLink(editor);
```

So the rules that make all the special behavior are also configurable.

TODO: insert code fragment of configuration lists

## Copy/paste of rendered volto-slate output

In the rendered output of special slate elements we include special `data-`
attributes so that when you copy rendered output and paste into slate blocks,
we shortcut the deserialization process and we reconstruct the original special
slate element. A rendered footnote, copy/pasted is not transformed to a simple
link, but is rebuilt as a footnote.

## ElementEditor

The element editor component provides an easy to use framework to write custom
plugins. For the "happy case", it is a factory function that only needs
a plugin id, a node renderer component and a schema for the node editing form.

See `volto-slate-dataentity` for a simplest example of a custom slate plugin
that introduces a new type of 'smart' element.

The ElementEditor also includes the ContextEditor toolbar, which is a toolbar
with two buttons that automatically pops up when the cursor is in a special
element: a button to edit the element and a button to delete the active
element.

The idea of the ElementEditor is to centralize all the repeating logic,
abstract the slate text editing specifics so that you can focus on your
specific element implementation. The centralization part is important,
I'd like to refactor some parts of it to improve its performance, and this way
we ensure we maintain compability with existing plugins.

## wrapInlineMarkupText

The benefit of implementing smart elements inside volto-slate is not only text
composition, but also text formatting. With volto-slate it is possible to copy
the markup styles applied to the placeholder text and apply them to the final
rendered output, no matter if it's a simple text or compound elements such as
a list.

## Are we there yet?

My feeling is that there's still a ton to improve and to get to a level of
parity with other editors. The editor is being used already by several
projects. It is an improvement over the default draftjs editor.

## Wishlist

- a complete HTML transformation story (soon to come)
- better, more normalization rules, maybe based on typed schema
- allow passing down a local configuration, for more specialized editors
- refactor the ElementEditor ContextEditor implementation (persistentHelpers)
- Quanta toolbar

## Migration to-from volto-slate

- Work has started on a Python-based html-to-json and json-to-html converters.
- Code exists already that migrates html to slate blocks, via nodejs external
  converter
