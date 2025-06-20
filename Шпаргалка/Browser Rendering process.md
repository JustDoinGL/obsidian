---
tags:
  - browser
---

## Parsing HTML

The browser parses our HTML and stores it in memory as a tree structure of a document, which is also known as DOM (Document Object Model) or sometimes as Real DOM. It is a Web API used to build websites.   

DOM methods allow programmatic access to the tree. With them, you can change the document’s structure, style, or content.
## Parsing CSS
The browser parses our CSS and stores it in memory as CSSOM (CSS Object Model). It is a Web API to manipulate the CSS of our website.
## Creating Render Tree

The browser uses DOM and CSSOM to create a render tree. Render Tree represents everything that will be rendered on the browser (HTML nodes with their styles).

## Layout Render Tree

Browser calculates the geometry of all elements (sizes & positioning) and accordingly starts placing them.

## Painting

Now it will start painting all individual nodes according to their styles.
