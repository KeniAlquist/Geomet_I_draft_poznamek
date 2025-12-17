---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 2
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

# Diferenciální struktura
#### D: Souřadnicová mapa $(U,\mathbf{x})$ v $M$
- $U$ otevřená oblast v M
- zobrazení $\mathbf{x}: U \to \mathbb{R}^{n}$
tj. dokážu se přenést na $\mathbb{R}^{n}$

#### D: Přechodové zobrazení
Mějme dvě zobrazení $(U,\mathbf{x})$ a $(V,\mathbf{y})$ na $M$. Pak definujeme zobrazení
$$
\begin{equation}
\begin{aligned}
\alpha := \mathbf{x}\mathbf{y}^{-1}: \mathbb{R}^{n}\to \mathbb{R}^{n}
\end{aligned}
\end{equation}
$$
tj. $\alpha$ operuje na $U \cap V$.
#### D: Atlas třídy $C^{r}$
Pro kolekce map $(U_{a},\mathbf{x_{a}})$.
- $U_{\alpha}$ tvoří otevřené pokrytí $M$
- přechodové zobrazení $\mathbf{x}_{ab}:=\mathbf{x_{a}}\circ \mathbf{x_{b}}^{-1}$ jsou tzv. třídy $C^{r}$.
$C^{0}$ - homeomorfizmus
$C^{r}$ - spojitých $r$-derivací
$C^{\infty}$ - nekonečně diferencovatelné
$C^{\omega}$ - analytické zobrazení
#### D: Kompatibilita atlasů
$A_{1},A_{2}$ jsou $C^{r}$-kompatibilní, pokud
$A_{1} \cup A_{2}$ je $C^{r}$ atlas.
#### D: Diferenciální struktura
Třída ekvivalence $C^{r}$-atlasů.
tj. i když existuje mnoho atlasů, my mezi nimi nerozlišujeme.
#### D: Maximální atlas
Sjednocení $\forall$ atlasů v diferenciální struktuře.
#### D: Diferencovatelná varieta $(M,A)$
Dvojice $(M,A)$ tj. topo. varieta s diferencovatelnou strukturou je tzv. diferencovatelná varieta třídy $C^{r}$.
