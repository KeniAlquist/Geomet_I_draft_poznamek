---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 1
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
# Topologické prostory

#### D: Topologický prostor $(X,\tau)$
je prostor $X$ s topologií $\tau$, tj souborem otevřených množin, takový že splňuje:
$$
\begin{equation}
\begin{aligned}
\tau: \, \emptyset,X &\in \tau \\
\bigcup_{i} A_{i} &\in \tau, \forall i \in I, A_{i}\in \tau \\
\bigcap_{i} A_{i} &\in \tau, \forall i \in I, A_{i}\in \tau
\end{aligned}
\end{equation}
$$
#### D: 
__Otevřená množina:__
je množina z $\tau$.
__Uzavřená množina:__
je komplement otevřené v $X$.
__Komponenta:__
zároveň uzavřená a otevřená množina.
__Báze topologie:__
soubor otevřených množin generující sjednocení a konečných průniků topologii.
__Kompaktní množina:__
Každé její pokrytí lze pokrýt konečně mnoha podpokrytími.

#### D: Spojité zobrazení $f$
Spojitým zobrazením $f:X\to Y$, topologických prostorů $X,Y$ splňující:
$$
\begin{equation}
\begin{aligned}
\forall x \in X \supset U,\quad \forall V\subset Y:\text{pro otevřené okolí } U \text{ bodu } x: f(U)\subset V
\end{aligned}
\end{equation}
$$


#### D: Homeomorfizmus
Vzájemně jednoznačné zobrazení $\phi$ a $\phi^{-1}$ jsou spojité.

## Charakterizace topologických prostorů
Vlastnosti pro nás důležité.
#### D: Hausdorffův prostor
Topologický prostor $M$, kde $\forall \mathscr x,\mathscr{y} \in M$, kde $\exists U, V \in \tau$ takové, že $\mathscr{x}\in U,\mathscr{y}\in V$ a $U\cap V=\emptyset$.

Další jiné:
- velikost: prostory lze generovat spočetnou bází.
- parakompaktnost: každé pokrytí má lokálně konečné podpokrytí.
#### D: Lokálně euklidovský
$\forall \mathscr{x}$ najdeme okolí, které je homomorfní s okolím v $\mathbb{R}^{n}$.
#### D: lokálně metrizovatelný
tzn. duh. sry. Vzdálenost mezi body můžu měřit.
#### D: Topologická varieta
$M$ je topologická varieta pokud:
- $M$ je lokálně euklidovský topologický prostor
- $M$ je Hausdorffovský
Dále: Lze pro ně ukázat 
- topo. varieta je disjunktní sjednocení souvislých komponent
- spočetná báze --> parakompaktnost
- parakompaktnost + spočetně komponent --> spočetná báze
- metrizovatelný $\iff$ parakompaktnost

