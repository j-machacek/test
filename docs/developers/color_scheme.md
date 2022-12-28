---
    layout: default
    title: Color scheme
    parent: For developers
---
# Color scheme

Per default [Just the docs]({% link https://just-the-docs.github.io/just-the-docs %}) uses its `lihght` scheme relying on purple and grey scales for displaying content. In order to adopt the coloring of 

1) Add the following conditional load stylesheets and scripts to the *head_custom.html* file:

```html
{ % if page.katex % }

<!-- CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@latest/dist/katex.min.css"/>

<!-- JavaScript -->
<script defer src="https://cdn.jsdelivr.net/npm/katex@latest/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@latest/dist/contrib/auto-render.min.js"
  onload="renderMathInElement(document.body,{
    delimiters: [
      { left: '$$',  right: '$$',  display: true  },
      { left: '$',   right: '$',   display: false },
      { left: '\\[', right: '\\]', display: true  },
      { left: '\\(', right: '\\)', display: false }
  ]});">
</script>

{ % endif % }
```
Note that the blanks between `%` and the brackets `{`, `}` in `{ % if page.katex % }`  and `{ % endif % }` must be removed!

2) To use LaTeX in a post, add `katex: true` to the YAML front matter, and write your LaTeX within the specified delimiters `$...$` or `\(...\)`. For instance, the body of the following:

```yaml
---
    layout: default
    title: Hypoplasticity + Intergranular Strain (Hypo+IGS)
    parent: Constitutive models
    katex: true
---
```