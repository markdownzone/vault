From notes under "dir" that have **Important Milestones** header at H3 level, display note link and content belonging to this header.

```js
const dir = dv.current().file.folder + "/ExampleFolder" // desired directory
const pages = dv.pages('"' + dir + '"');
const headerLevel = "### ";
const headerText = "Important Milestones";
const result = [];
// returns true if "line" belongs to the given "level"
const isHigherLevel = (line, level) => {
	let result = !(line.startsWith(level));
	for (let i = 1; i < (level.length - 1); i++) result = result && !(line.startsWith(level.slice(i)));
	return result;
}
// for each page, check each line in the page then push the content under desired header to result
for (let [index, page] of Object.entries(pages.values)) {
	const content  = await dv.io.load(page.file.path);
	const lines = content.split("\n");
	let desiredLines = "";
	let isHeaderLine = false;
	for (const line of lines) {
		if (line.startsWith(headerLevel) && line.endsWith(headerText)) {
			isHeaderLine = true;
            desiredLines += "<u>" + line + "</u>" + "<br>";
			continue;
		}
		// if line belongs to desired header
		if (isHeaderLine && isHigherLevel(line, headerLevel)) {
			desiredLines += line + "<br>";
			continue;
		} else {
			isHeaderLine = false;
			continue;
		}
	}
	result.push({"page": page, "content": desiredLines});
}


dv.table(["Link", "Important Milestones", "Created Date"], 
    dv.array(result).sort(obj => obj.page.file.ctime, "desc") // desired sort method
	.map(obj => [dv.fileLink(obj.page.file.path), obj.content, obj.page.file.ctime])
);

this.container.querySelectorAll(".table-view-table").forEach(s => s.style.width = "800px");
```