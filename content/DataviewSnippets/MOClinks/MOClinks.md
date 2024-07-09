```js
// construct a list of strings in format dir/subdir/subdir.md
const paths = dv.current().file.path.split("/").slice(0,-1);
let dirPath = "";

for (let i = 0; i < paths.length; i++) {
	dirPath += paths[i] + "/";
	paths[i] = dirPath + paths[i] + ".md";
}
// add the note at path to the list if it has its moc property as true, otherwise add a string saying dir has no MOC file
const mocFiles = [];
for (const path of paths) {
	const mocFile = app.vault.getAbstractFileByPath(path);
	if (!mocFile || !(app.metadataCache.getFileCache(mocFile)?.frontmatter?.moc || false)) {
		mocFiles.push(path.slice(0, path.lastIndexOf("/")) + " has no MOC file.");
		continue;
	} else {
		mocFiles.push(mocFile);
	}
}
// display the MOC in file's folder outside of the callout and if it doesn't exist, link to Uncategorized notes.
// display links as a callout, in descending order
dv.paragraph("<span class=\"type-link\">" + dv.fileLink(mocFiles.pop()?.path || "FleetingNotes/Uncategorized/Uncategorized.md") +"</span>\n");
mocFiles.reverse();
let links = "";
for (const mocFile of mocFiles) {
	if (typeof mocFile == "string") {
		links += "> " + mocFile +" \n";
		continue;
	}
	links += "> " + dv.fileLink(mocFile.path) + "\n";
}
dv.paragraph("> [!link|map]- MOC Links\n" + links);
```