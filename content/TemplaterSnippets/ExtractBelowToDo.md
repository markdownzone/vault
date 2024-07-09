<%*
const section = {}; // positions of the section we want to extract
const prevDailyNote = app.vault.getAbstractFileByPath("Archive/TemplaterSnippets/test note.md")
const prevCache = app.metadataCache.getFileCache(prevDailyNote);
const prevToDo = prevCache?.headings.filter((h) => h.heading == "To-do");

section.start = prevToDo[0].position.end // To-do header's end is where section starts
// if another section with same or lower level than To-do, make it as section's end position
section.end = prevCache?.headings.filter(
  (h) => h.position.start.line > section.start.line && h.level <= prevToDo[0].level
)[0]?.position.start;
// otherwise make document's end as section's end
if (section.end == undefined) {
  Object.assign(section, {end: prevCache?.sections[prevCache?.sections.length-1].position.end})
}

// split previous daily's text and extract the desired content
const prevDailyContent = (await app.vault.cachedRead(prevDailyNote)).split("\n");
let content = "";
for (let i = section.start.line; i < section.end.line; i++) {
  content += prevDailyContent[i] + "\n";
}

// modify further or display the extracted content
tR += content;
%>