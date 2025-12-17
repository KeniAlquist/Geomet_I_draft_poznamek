---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 3
---

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
        const parentTagParts = tagParts.slice(0, -1);
        const parentTagPath = parentTagParts.join("/");

        const parentFolderName = parentTagParts[parentTagParts.length - 1];
        parentPath = `Main/${parentFolderName}`;
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
        (prevNote ? `[[${prevNote.file.name}|<< prev]]` : "<< prev") + " | " +
        (showUp ? `[[${parentPath}|up]]` : "up") + " | " +
        (nextNote ? `[[${nextNote.file.name}|next >>]]` : "next >>")
    );
}
```

# Hladké zobrazení
#### D: Hladké zobrazení mezi $M$ a $N$
Zobrazení $\phi:M\to N$ je $C^{r}$, pokud
$\forall (U,\mathbf{x})$ na $M$ a $(V,\mathbf{y})$ na $N$ bude souřadnicové vyjádření $\phi$ třídy $C^{r}$ tj.
$$
\begin{equation}
\begin{aligned}
\mathbf{y}\circ\phi\circ \mathbf{x}^{-1}:\mathbb{R}^{m}\to \mathbb{R}^{n} \text{ je } C^{r}
\end{aligned}
\end{equation}
$$
Obrázkově
![[Drawing 2025-10-02 10.16.47.excalidraw.png|center|700]]
#### D: Difeomorfizmus
$\phi: M\to N$ je $C^{r}$ difeomorfizmus. Pokud $\exists \phi^{-1}$ a $\phi, \phi^{-1}$ jsou $C^{r}$ hladké.
(Prostor difeomorfizmů ozn. $\text{Diff}(M,N)$)

## Speciální případy
#### D: Hladká funkce
$f: M\to \mathbb{R}$; ozn. souřadnicově $\bar{f}(u^{1},\dots,u^{m}): \mathbb{R}^{n}\to \mathbb{R}$
#### D: Hladké křivky
$\gamma:\mathbb{R}\to M$; ozn. souřadnicově $\bar{\gamma}^{k}:\mathbb{R}\to \mathbb{R}^{n}$
#### D: Difeoekvivalentní variety
$(M,A_{M})$ a $(N,A_{N})$ jsou difeoekvivalentní $\iff$ $\exists$ difeomorfizmus $\phi:M\to N$

