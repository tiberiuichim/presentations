---
class:
  - invert
headingDivider: 2
theme: gaia
style: |
---
  <style>
    .hljs-name {
      color: #7ee787 !important;
    }

    .hljs-tag {
      color: #79c0ff !important;
    }

    h2 {
      font-family: serif;
    }
    section.leftbg h2 {
      text-align: left !important;
    }

    .leftbg h2 {
    }

    section {
      font-size: 1.6rem;
    }
    section.invert {
      --color-background: #175E58!important;
      padding: 2em 3em !important;
    }

    section code {
      color: #c9d1d9 !important;
      background: #161b22 !important;
    }
    img {
      max-height: 80vh;
      max-width: 100%;
      text-align: center;
    }
    a { color: white }
  </style>

<!-- _class: lead invert

backgroundImage: linear-gradient(to bottom, #00228A, #086ca3)
-->

<!--

steps, practical examples

we want 3 things, like EC design system:

- usage
- showcase
- storybook

maintain the design system, main components, we import the release


sui code, react components, releasable, we can import in our volto theme setup and they must have a Plone

show a presentation of how to get started.

show a workflow diagram on how they should work.

components library exist, we don't have the design system, that's what's inside react.

show them examples, they have to do.

we want to deliver the specification for them.

- how they should develop this,
- how they should deliver this
- how they should maintain the library, they have to be maintainers, it's their main delivery, we have to establish a colaboration system for back/forth.

- technical requirements ambition level. Get a feeling from their team.

- what we want from the design system. Show EC example, explain instead of that code we want SemanticUI code, the contents are almost the same.

- We need things releasable, so we want a library with a theme and components. Development workflow with a diagram on how they deliver, how we use them.

- We're gonna have a test site. Practical 'how to deliver, how to develop'. Document how to make the documentation.

- Already schedule some more in-depth meetings for specific things: how to do stuff with Volto.

====] Timeline of meeting, Agenda: [====

- short intro from Antonio + Marie
    - they need to work with us
    - expected delivery + deadlines
- short intro on the projects we do. Show Volto websites
- theming system in Volto (semanticui, themes, react components)
- design system details
- development workflow. Document the documentation.
- discuss the need for test site
- schedule new meetings for specific sites
-->




# EEA Design System

<!-- _class: lead invert -->

# EEA CMS

- We use Plone 6 CMS
- Everything is open source
- React frontend (Volto)

# The websites

- EEA IMSv4
  https://www.eea.europa.eu/ims
- FISE
  https://forest.eea.europa.eu/
- BISE
  http://biodiversity.europa.eu/
- WISE-Freshwater
  https://water.europa.eu/freshwater
- Climate-Energy
  https://climate-energy.eea.europa.eu/
- Industrial Emissions
  https://industry.eea.europa.eu/
- more to come

# EEA editing process

- WYSIWYG editing
- Pages are composite pages, text intermixed with special blocks
- System similar to Wordpress Gutenberg

# Volto theming system

- SemanticUI CSS framework
- SemanticUI-React for components
- custom EEA addons
- Each website has its own Volto theme. We want to change that

# The future EEA Design System

**Creation and maintenance of a design system:**

- Branding, high level specification
- Complete design system as a static website
- Integrate SemanticUI + React
- Iconography
- Design Kits
- Github repos

See:

- Europa Component Library https://ec.europa.eu/component-library/ec/
https://github.com/ec-europa/europa-component-library
- Awesome Design Systems https://github.com/alexpate/awesome-design-systems

# Deliveries

- Requirement 1 — **Creation of a high-level branding** (1 Jan)
- Requirement 2 — **Core Semantic UI theme** (End-Jan 22)
- Requirement 3 — **All design elements** (End-Feb 22)

## Requirement 1 - High level branding

Creation of a high-level branding concept and design principles for the EEA and
its family of websites, taking into consideration pre-existing work (EEA logo,
corporate color palette) (see annex 10), design of EEA printed products),
current web design elements.

Core template (site header, page header, footer) e.g.,
https://ec.europa.eu/component-library/ec/core-template/

##

This would also include

- Colors
- Typography
- Main grid

Not in scope in this package:

- branding of printed material, e.g. reports, posters.

## Applied branding

Specific example of branding applied to an **EEA standard webpage** (e.g.
article/briefing):

- Layout example of a webpage with applied template, in 3 main layouts:
  - Mobile (vertical)
  - Tablet/mobile-landscape (landscape)
  - Desktop

## Mobile first

Design proposals should start with the smallest resolution and viewport in mind
(mobile-first), and progressively create layouts for the next resolution and so
on (progressive enhancements).

**Mobile**: Provide a design for mobile displays with size 360×640 (this will help
us create a fluid design from 360×640 through 414×896)

**Tablet**: Provide a design for tablet displays by giving a design with size
768×1024 (this will help us create a fluid design from 601×962 through
1280×800)

**Desktop**: Provide a design for desktop displays by giving a design with size
1366×768 (this will help us create a fluid design from 1024×768 through
1920×1080)

## Delivery of requirement 1

**Expected delivery to EEA web team: 1 Jan 2022**

Deliver approved Figma (or similar) layouts


<!--

On 1 Jan, web team should receive header, page header, footer

-->

## Requirement 2 — Core Semantic-UI theme (End-Jan 22)

Branding applied to Semantic UI core elements - https://react.semantic-ui.com/

## Design system as static website

- Based on these new branding and design principles, implementation of
  a complete online design system. The design system is to be published as
  a website under the EEA domain, e.g., https://design.eea.europa.eu (using
  Docusaurus and Storybook) documenting and showcasing all elements of the new
  design. Its content is expected to be similar to
  https://ec.europa.eu/component-library/ec/ with EEA specific needs.

## Semantic-UI Theme and React components

- The design system will be based on specific Semantic UI React-components
  (https://react.semantic-ui.com/elements/header/), that will need to be styled
  according to the EEA design systems and then re-used by designers and web
  developers working in other projects/contracts at EEA.

## Accessibility

- Highly accesible implementation of components is expected. See WCAG 2.0
(A/AA/AAA) accessibility standards and requirements

## Iconography as SVG

- Iconography for the new design system, in vector format as SVG icons library.

## Downloadable design kits

- The design system should not only address the needs of web developers but
  also address the needs of designers by including downloadable “Design Kits”
  in Adobe Illustrator, see examples on some existing design systems for
  example “Aurora Design System”.

## Github repos

- All the source code and design system deliverables will be managed in
  specific repos in EEA GitHub (https://github.com/eea) and design system
  website hosted at EEA.

## Maintainance, further development of the design system

- Maintain and develop further the design system
- Integrate with other systems (EEA Semantic Search)

## Resource people

- David Ichim, main EEA website frontend developer
  - EEA website expert
- Tiberiu Ichim, lead developer for thematic websites
  - Volto systems expert
- Backup: Alin Voinea, lead developer EEA website
  - Development process expert
