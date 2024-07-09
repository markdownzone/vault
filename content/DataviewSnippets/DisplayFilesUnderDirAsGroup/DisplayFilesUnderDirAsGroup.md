```js
for (let group of dv.pages('"Archive"').groupBy(p => p.file.folder)) {
  let filePath = String(group.rows.file.folder[0]);
  if (filePath == "") filePath = "Root";
  dv.table([filePath, "Created", "Modified"],
    group.rows
    .map(k => [k.file.link, k.file.ctime, k.file.mtime])
  );
}

this.container.style["width"] = "fit-content";
```