### Default width

```js
const dir = dv.current().file.folder + "/ExampleFolder" // desired directory
dv.table(["Subtasks", "File", "Created Date"],
	dv.pages('"' + dir + '"').map(t => 
		[t.file.tasks.array().length - 1, t.file.link, t.file.ctime]
	)
)
```

### Width = 'fit-content'

```js
const dir = dv.current().file.folder + "/ExampleFolder" // desired directory
dv.table(["Subtasks", "File", "Created Date"],
	dv.pages('"' + dir + '"').map(t => 
		[t.file.tasks.array().length - 1, t.file.link, t.file.ctime]
	)
)


this.container.querySelectorAll(".table-view-table").forEach(s => s.style.width = "fit-content");
```