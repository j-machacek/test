---
    layout: default
    title: Math
    parent: For developers
---
# Math

Per default the `kramdown` parser used by `Jekyll` does not allow for inline math display. To enable inline math display, some small changes to the html `<head>` tag are required. Fortunately, `just-the-docs` has a dedicated *head_custom.html* file that can be used for this purpose. This file is located in *_includes/* and the necessary changes have already been made. If a new version of just-the-docs requires a new adjustment, the necessary steps are explained again below:

1) Add the following conditional load stylesheets and scripts to the *head_custom.html* file:

```html
IF CLAUSE

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

END IF CLAUSE
```
where:
```text
IF CLAUSE = { % if page.katex % }
```
and 
```text
END IF CLAUSE = { % endif % }
```

2) To use LaTeX in a post, add `katex: true` to the YAML front matter, and write your LaTeX within the specified delimiters `$...$` or `\(...\)`. For instance, the body of the following:

```yaml
---
    layout: default
    title: Hypoplasticity + Intergranular Strain (Hypo+IGS)
    parent: Constitutive models
    katex: true
---
```