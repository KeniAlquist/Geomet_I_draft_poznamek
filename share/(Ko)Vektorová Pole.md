---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 5
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

# Vektorová pole a pole forem
#### D: Prostor vektorových polí $\mathcal{T}_{}M$
Prostor vektorových polí $a$, které jsou zobrazení $a:M\to \mathbf{T}_{}M$. (Lze uvažovat jako řez bandlů)

#### D: Prostor kovektorových polí $\mathcal{T}^{*}_{}M$
Prostor kovektorových polí $\alpha$, které jsou zobrazení $\alpha: M\to\mathbf{T}_{}^*M$.

**Poznámky:** 
- Obě zobrazení jsou hladké pakliže jsou komponenty hladké.
- $\mathcal{T}_{}M$ a $\mathcal{T}_{}^*M$ jsou "reflexivní" neboli duální.
- $\mathcal{T}_{}M$ a $\mathcal{T}^*M$ jsou moduly nad okruhem $\mathcal{F}_{}M$, tehdy se mluví o $\mathcal{F}_{}M$-linearitě (Těleso: "skaláry" jsou funkce).
- Reflexivita má za důsledek předvídatelnou vlastnost:
$$
\begin{equation}
\begin{aligned}
m:\mathcal{T}_{}^*M\to \mathcal{F}M\\
\mu:\mathcal{T}_{}M\to \mathcal{F}M
\end{aligned}
\end{equation}
$$
obě $\mathcal{F}M$-lineární. $fm[\alpha+\beta]=m[f\alpha]+m[f\beta]$, Pak z reflexivity.: $m\in \mathcal{T}_{}M,\mu\in\mathcal{T}_{}^*M$.

#### D: Derivace na okruhu $\mathcal{F}M$
Zobrazení $a: \mathcal{F}M\to \mathcal{F}M$, splňující:
$$
\begin{equation}
\begin{aligned}
a[f+g]&=a[f]+a[g]\\
a[rf]&=ra[f]\\
a[fg]&=fa[g]+a[f]~g
\end{aligned}
\end{equation}
$$
(takové zobrazení je ekvivalentní vektorovému poli na $M$, pak $a[f]|_{\mathscr{x}}$ určuje derivaci v $\mathscr{x}$ $a|_{\mathscr{x}}\in \mathbf{T}_{\mathscr{x}}M$)

#### D: Lieova závorka
$$
\begin{equation}
\begin{aligned}
\ [\cdot{},\cdot]:\mathcal{T}_{}M\times \mathcal{T}_{}M\to \mathcal{T}_{}M
\end{aligned}
\end{equation}
$$
Působení závorky porozumíme aplikace na funkcích.
$$
\begin{equation}
\begin{aligned}
\bigl[a,b\bigr][f]&= a\bigl[b[f]\bigr]-b\bigl[a[f]\bigr]\\
\\
\bigl[a,b\bigr][fg]&= a\bigl[b[f]g\bigr]+fa\bigl[b[g]\bigr]-b\bigl[a[f]g\bigr]-b\bigl[fa[g]\bigr] = f\bigl[a,b\bigr][g]+\bigl[a,b\bigr][f]g
\end{aligned}
\end{equation}
$$
Vidíme, že splňuje definici derivace, zde na $\mathcal{F}M$.

__Vlastnosti:__
- $[a,b]=-[b,a]$
- $[a,fb]=f[a,b]+a[g]b$
- $[fa,b]=f[a,b]-b[f]a$
- $\bigl[a,[b,c]\bigr]+\bigl[b,[c,a]\bigr]+\bigl[c,[a,b]\bigr]=0$

#### D: Holonomní báze
Hovoříme o bázi $e_{j}$ jako o holonomní, pokud $\exists x^{j}: e_{j}=\partial_{x^{j}}$.

#### D: $\mathbf{T}_{}M$ a Paralizovatelnost
$\mathbf{T}_{}M$ je paralizovatelná $\iff$ $\exists$ globální báze