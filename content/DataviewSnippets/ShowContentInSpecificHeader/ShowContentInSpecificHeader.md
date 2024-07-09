
From samples notes that have **Events** header at H3 level and **Observations** header at H4 level, display note link and content belonging to these headers.

```js
const dir = dv.current().file.folder + "/ExampleFolder" // desired directory
const pages = dv.pages('"' + dir + '"');
const evtLevel = "### ";
const obsLevel = "#### ";
const result = [];
// returns true if @line belongs to the given @level
const isHigherLevel = (line, level) => {
	let result = !(line.startsWith(level));
	for (let i = 1; i < (level.length - 1); i++) result = result && !(line.startsWith(level.slice(i)));
	return result;
}
// for each page, check each line in the page then push the content under Events and Observation headers to result
for (let [index, page] of Object.entries(pages.values)) {
	const content  = await dv.io.load(page.file.path);
	const lines = content.split("\n");
	let evtLines = "";
	let obsLines = "";
	let isEvtLine = false;
	let isObsLine = false;
	for (const line of lines) {
		if (line.startsWith(evtLevel) && line.endsWith("Events")) {
			isEvtLine = true;
			continue;
		}
		if (isEvtLine && line.startsWith(obsLevel) && line.endsWith("Observations")) {
			isObsLine = true;
			continue;
		}
		// if line belongs to Observation header
		if (isObsLine && isHigherLevel(line, obsLevel)) {
			obsLines += line + "<br>";
			continue;
		}
		// if line belongs to Event header
		else if(isEvtLine && isHigherLevel(line, evtLevel)) {
			isObsLine = false;
			evtLines += line + "<br>";
			continue;
		} else {
			isEvtLine = false;
			continue;
		}
	}
	result.push({"page": page, "event": evtLines, "observation": obsLines});
}


dv.table(["Link", "Events", "Observation", "Created Date"], dv.array(result).sort(obj => obj.page.file.ctime, "desc") // desired sort method
	.map(obj => [dv.fileLink(obj.page.file.path), obj.event, obj.observation, obj.page.file.ctime])
);

this.container.querySelectorAll(".table-view-table").forEach(s => s.style.width = "1800px");
```