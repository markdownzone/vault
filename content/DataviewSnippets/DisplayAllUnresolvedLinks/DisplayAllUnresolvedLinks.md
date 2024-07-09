Finds all unresolved links except images canvases and stuff, because they show up as unresolved even if they exist

```js
// DataviewJS query to list pages with links to non-existent files, excluding images, canvas, mp4 and PDFs
dv.pages()
  .where(page => page.file.outlinks.length > 0)
  .flatMap(page => page.file.outlinks
    .filter(outlink => !dv.page(outlink.path) && !/\.(jpg|jpeg|png|gif|pdf|webp|canvas|mp4)$/i.test(outlink.path))
    .map(outlink => [page.file.link, outlink.path]))
  .forEach(([page, outlink]) => {
    dv.paragraph(`${page} links to non-existent file: [[${outlink}]]`);
  });
```
