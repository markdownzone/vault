### Show all tasks and their subtasks

```dataview
TABLE WITHOUT ID
file.link AS "File",
regexreplace(Tasks.text, "\[.*$", "") AS Task,
choice(Tasks.completed, "🟢", "🔴") AS Status,
regexreplace(Tasks.subtasks.text, "\[.*$", "") AS Subtasks,
choice(Tasks.subtasks.completed, "🟢", "🔴") AS "Subtasks Status",
file.ctime AS "Created Date"
FROM "Archive/DataviewSnippets/ShowTasksInTable/ExampleFolder"
FLATTEN file.tasks AS Tasks
//WHERE Tasks.parent = null
SORT asd asc
```

### Only show tasks at level 0 and their subtasks

```dataview
TABLE WITHOUT ID 
file.link AS "File",
regexreplace(Tasks.text, "\[.*$", "") AS Task, 
choice(Tasks.completed, "🟢", "🔴") AS Status, 
Tasks.subtasks.text AS Subtasks, 
choice(
	length(Tasks.subtasks) = 0, "⚪",
	choice(
		Tasks.subtasks.completed, "🟢", "🔴"
	)
) AS "Subtasks Status",
file.cday AS "Created Date"
FROM "Archive/DataviewSnippets/ShowTasksInTable/ExampleFolder"
FLATTEN file.tasks AS Tasks
WHERE Tasks.parent = null
SORT file.ctime asc
```