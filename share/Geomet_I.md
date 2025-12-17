---
tags:
  - Magistr/Geomet/Geomet_I
Semestr: 1
---
# Geometrické metody 1
```dataviewjs
const currentFile = dv.current();
const allTags = currentFile.tags || [];

if (allTags.length === 0) {
    dv.paragraph("⚠ No valid tag found!");
} else {
    const tag = allTags.sort((a, b) => b.length - a.length)[0];
    const tagParts = tag.split("/");

    let peerNotes = [];
    let parentPath = "";
    let showUp = false;

    if (tagParts.length > 1) {
        // Extract parent tag path: one level up
        const parentTagParts = tagParts.slice(0, -1);
        const parentTagPath = parentTagParts.join("/");

        // Folder for "up" button
        const parentFolderName = parentTagParts[parentTagParts.length - 1];
        parentPath = `Main/${parentFolderName}`;
        showUp = true;

        // Match siblings at current depth
        peerNotes = dv.pages()
            .where(n => n.Téma && n.tags && n.tags.some(t => {
                const tParts = t.split("/");
                return (
                    tParts.length === tagParts.length &&
                    tParts.slice(0, -1).join("/") === parentTagPath
                );
            }))
            .sort(n => n.Téma, 'asc');
    } else {
        // Top-level tag (no parent)
        peerNotes = dv.pages()
            .where(n => n.Téma && n.tags && n.tags.some(t => t.split("/").length === 1))
            .sort(n => n.Téma, 'asc');
    }

    const currentIndex = peerNotes.findIndex(n => n.file.name === currentFile.file.name);
    const prevNote = currentIndex > 0 ? peerNotes[currentIndex - 1] : null;
    const nextNote = currentIndex < peerNotes.length - 1 ? peerNotes[currentIndex + 1] : null;

    dv.paragraph(
        (prevNote ? `[[${prevNote.file.name}|<< prev]]` : "<< prev") + " | " +
        (showUp ? `[[${parentPath}|up]]` : "up") + " | " +
        (nextNote ? `[[${nextNote.file.name}|next >>]]` : "next >>")
    );
}

```
```dataview
TABLE Téma AS "č."
FLATTEN filter(
  tags,
  (t) => regexmatch("^Magistr/Geomet/Geomet_I/[^/]+$", t)
)[0] AS FullTag
WHERE FullTag != null AND Téma != null
SORT Téma

```

```dataviewjs
const currentFile = dv.current();
if (!currentFile) {
    dv.span("Error: No active file found.");
} else {
    const fileName = currentFile.file.name.replace(/\.md$/, "");
    const tags = currentFile.tags || [];

    if (tags.length === 0) {
        dv.span("Error: No tags found in this note.");
    } else {
        const primaryTag = tags.sort((a, b) => b.length - a.length)[0];
        const newTag = `${primaryTag}/${fileName}`;

        const button = dv.el("button", "Další téma");
        button.style.padding = "8px 16px";
        button.style.backgroundColor = "#007acc";
        button.style.color = "white";
        button.style.border = "none";
        button.style.borderRadius = "4px";
        button.style.cursor = "pointer";

        button.onclick = async () => {
            const newNoteName = `Název`; // Adjust as needed
            const newFilePath = `Main/${newNoteName}.md`;

            const navigationScript = `\`\`\`dataviewjs
const currentFile = dv.current();
const allTags = currentFile.tags || [];

if (allTags.length === 0) {
    dv.paragraph("⚠ No valid tag found!");
} else {
    const tag = allTags.sort((a, b) => b.length - a.length)[0];
    const tagParts = tag.split("/");

    let peerNotes = [];
    let parentPath = "";
    let showUp = false;

    if (tagParts.length > 1) {
        const parentTagParts = tagParts.slice(0, -1);
        const parentTagPath = parentTagParts.join("/");

        const parentFolderName = parentTagParts[parentTagParts.length - 1];
        parentPath = \`Main/\${parentFolderName}\`;
        showUp = true;

        peerNotes = dv.pages()
            .where(n => n.Téma && n.tags && n.tags.some(t => {
                const tParts = t.split("/");
                return (
                    tParts.length === tagParts.length &&
                    tParts.slice(0, -1).join("/") === parentTagPath
                );
            }))
            .sort(n => n.Téma, 'asc');
    } else {
        peerNotes = dv.pages()
            .where(n => n.Téma && n.tags && n.tags.some(t => t.split("/").length === 1))
            .sort(n => n.Téma, 'asc');
    }

    const currentIndex = peerNotes.findIndex(n => n.file.name === currentFile.file.name);
    const prevNote = currentIndex > 0 ? peerNotes[currentIndex - 1] : null;
    const nextNote = currentIndex < peerNotes.length - 1 ? peerNotes[currentIndex + 1] : null;

    dv.paragraph(
        (prevNote ? \`[[\${prevNote.file.name}|<< prev]]\` : "<< prev") + " | " +
        (showUp ? \`[[\${parentPath}|up]]\` : "up") + " | " +
        (nextNote ? \`[[\${nextNote.file.name}|next >>]]\` : "next >>")
    );
}
\`\`\``;

            const newNoteContent = `---
tags:
  - ${newTag}
Téma: 
---

${navigationScript}

# ${newNoteName}
`;

            try {
                const existingFile = app.vault.getAbstractFileByPath(newFilePath);
                if (existingFile) {
                    new Notice(`Note "\${newNoteName}" already exists!`);
                    return;
                }

                const newFile = await app.vault.create(newFilePath, newNoteContent);
                if (newFile) {
                    await app.workspace.getLeaf().openFile(newFile);
                    new Notice(`Created and opened: \${newNoteName}`);
                }
            } catch (error) {
                new Notice(`Error creating note: \${error.message}`);
                console.error("Error creating note:", error);
            }
        };

        dv.container.appendChild(button);
    }
}

```
https://utf.mff.cuni.cz/vyuka/NTMF059/