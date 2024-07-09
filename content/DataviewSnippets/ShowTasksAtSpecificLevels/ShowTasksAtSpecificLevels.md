### Shows tasks at level 1 and level 2 only

```js
const dir = dv.current().file.folder + "/ExampleFolder" // desired directory
const myTasks = dv.pages('"' + dir + '"').file.tasks 
	.filter(t => t.parent == null || t.parent < 2).map(t => {
		if (t.parent == null) return t;
		if (t.parent != null) {
			t.children = [];
			return t;
		}
	})
dv.taskList(myTasks, false);
```

### Shows All tasks

```js
dv.taskList(dv.pages('"' + dv.current().file.folder + "/ExampleFolder" + '"').file.tasks, false);
```

