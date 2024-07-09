### Split dataview columns into two

```js
const pages = dv.pages().filter(p => dv.func.contains(p.tags, "föreläsning")).filter(p => p.course == "Linjär algebra").sort(f => f.sortOrd, "asc");
const evens = [];
const odds = [];
for (let [index, val] of pages.array().entries()){
	if (index%2 == 0) evens.push(val);
	if (index%2 != 0) odds.push(val);
}
const calloutTop = "> [!multi-column]\n> \n";
const calloutBody = "> > [!blank|min-0]\n> > ";

dv.paragraph("Notes:");

dv.paragraph(calloutTop + calloutBody 
	+ dv.markdownTable(["Notes"], 
		evens.map(p => [p.file.link])) 
	+ "> \n" + calloutBody 
	+ dv.markdownTable(["Notes"], 
		odds.map(p => [p.file.link]))
);
```