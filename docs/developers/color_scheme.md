---
    layout: default
    title: Color scheme
    parent: For developers
---
# Color scheme

Per default [Just the docs](https://just-the-docs.github.io/just-the-docs) uses its `light` scheme relying on purple and grey scales for displaying content. In order to adopt the colouring of **numgeo-ACT** we changed the default settings. If this needs to be repeated or adjustments are required, these are the necessary steps: 

1) Add the directory *_sass* to the main directory of the project 

2) Add the subdirectory *color_schemes*  

3) Create a new file named *numgeoACT_light.css* in *color_schemes* with the following content: 

```css 
// color for numgeo logo: rgb(47,46,66)
// color for ACT logo: rgb(150,28,68)

// Background
$body-background-color: #ffffff;
$sidebar-color: $grey-lt-100;
$search-background-color: $grey-lt-100;

// Lines
$border-color: $grey-lt-100;

// Text
$body-text-color: $grey-dk-100;
$body-heading-color: rgb(47,46,66);
$nav-child-link-color: $grey-dk-100;
$search-result-preview-color: $grey-dk-100;
$link-color: rgb(150,28,68);

// Buttons
$btn-primary-color: rgb(150,28,68);
$base-button-color: $grey-lt-000;

// Tables
$table-background-color: $grey-lt-000;
$feedback-color: darken($sidebar-color, 3%);

@import   "./color_schemes/light";

// Code
$code-background-color: $grey-lt-000;
```