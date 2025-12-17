---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 9
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

# Uzavřené a exaktní formy
### D: Uzavřená a Exaktní forma
$\omega$ je uzavřená $\iff$ $\mathrm{~d}\omega=0$ ; Prostor _uzavřených_ $\mathcal{A}_{\mathscr{c}}^{P}M=\text{Ker}[\mathrm{d}]$
$\omega$ je exaktní $\iff$ $\exists\sigma:\omega=\mathrm{~d}\sigma$ ; Prostor _exaktních_ $\mathcal{A}_{\mathscr{e}}^{P}M=\text{Img}[\mathrm{d}]$

přirozeně _exaktní_ $\implies$ _uzavřená_
### D: de Rhamova kohomologie
$\mathcal{H}^{P}(M)=\mathcal{A}^{P}_{\mathscr{c}}M/\mathcal{A}_{\mathscr{e}}^{P}M$
neboli pro $\omega$ formy je to třída ekvivalence$(\omega)$, invariantní vůči přičtení exaktních forem $\mathrm{d}\sigma$: $(\omega)=\omega+\mathrm{d}\sigma$

### D: Bettiho číslo
Dimenze de Rhamovy kohomologie. $b^{p}(M)=\text{dim}(H^{P}(M))$. 
Taky představuje maximální počet řezů (dané dimenze $p$), které je třeba provést, abychom varietu rozsekali na 2 kusy.

Má souvislost s _Eulerovou charakteristikou:_ $\chi(M):=\sum_{p=0,\dots ,d}~(-1)^{p}~b^{p}(M)$.

### Lemma: Poincarého lemma
pro topologicky triviální varietu $M$ (bez děr) platí:
$\omega\text{ uzavřená} \implies \omega\text{ exaktní}$. Tj. Betti $=1=b^{0}(M);~b^{p}(M)=0,~p>0$.

__Příklady:__
- $S^{n}$ má $b^{n}=b^{0}=1$.
- $T^{2}$ má $b^{0}=1,b^{1}=2,b^{2}=1$
(lze interpretovat jako: $b^{0}:\#$ spojitých komponent, $b^{1}:\#$ kruhových smyček (1D), $b^{2}:\#$kruhových děr (2D), etc...)

### Holonomie báze
(i) $e_{k}$ je holonomní, tj. $\exists x^{k}: e_{k}=\frac{ \partial  }{ \partial x^{k} }$
$\iff$
(ii) $e^{k}$ jsou exaktní, tj. $\exists x^{k}: e^{k}=\mathrm{d}x^{k}$
$\iff$
(iii) $\mathrm{d}e^{k}=0,\forall k$ na topologicky triviální oblasti.
$\iff$
(iv) $\ [e_{k};e_{l}]=0,\forall k,l$

DK: (iii)$\iff$(iv) nejméně triviální:
Uvažme Cartanův vzorec
$$
\begin{equation}
\begin{aligned}
e_{k}\cdot \mathrm{d}e^{m}\cdot e_{l}
&=e_{k} \cdot \mathrm{~d}(e^{m}\cdot e_{l})-\mathrm{~d}(e_{k}\cdot e^{m})\cdot e_{l}-[e_{k};e_{l}]\cdot e^{m}\\
&=0-0-[e_{k};e_{l}]\cdot e^{m}
\end{aligned}
\end{equation}
$$
$\mathrm{~d}e^{m}=0\iff[e_{k};e_{l}]=0$ pro všechna $m$.


