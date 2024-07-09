```js
const parentNotes = dv.pages("#desiredTag").array();

const rows = [];
const columns = ["Note"];
const level = 8;

function recursiveOutlinks(addNote = false) {
    if (addNote) rows.push([parentNotes.shift().file.link]);
    for (const row of rows) {
        if (row.length < level + 1) {
            if (dv.page(row.at(-1)).file.outlinks.length == 0) {
                for (let i = row.length; i < level + 1; i++) row.push("no outlinks");
                return recursiveOutlinks();
            }
            for (const link of dv.page(row.at(-1)).file.outlinks) rows.push([...row, link])
            rows.splice(rows.indexOf(row), 1);
            return recursiveOutlinks();
        }
    }
    return;
}

for (const page in parentNotes) recursiveOutlinks(true);
for (let i = 0; i < level; i ++) columns.push("Outlinks Level " + (i + 1));

dv.table(columns, rows);
```

