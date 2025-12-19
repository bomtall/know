
To create a dataset of SVG that can be used in Power BI open the SVG file in a text editor:

- Remove the following text from the start:

```
<?xml version="1.0" encoding="UTF-8"?>
```

- Add the following to the start:

```
data:image/svg+xml;utf8,
```

- Put all of the text onto one line:

```
data:image/svg+xml;utf8,<svg id="Vectorized_ikon" data-name="Vectorized ikon" xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 0 32 32"><defs><style>.cls-1 {fill: none;}.cls-1, .cls-2 {stroke-width: 0px;}.cls-2 {fill: #333333;}</style></defs><rect class="cls-1" width="32" height="32"/><polygon class="cls-2" points="23 6 23 21.5859 7.707 6.293 6.293 7.707 21.5859 23 6 23 6 25 25 25 25 6 23 6"/></svg>|
```