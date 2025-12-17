---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 18
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

# Klasifikace kovariantních derivací
Všechny dosavadní [[Kovariantní Derivace|kovariantní derivace]] si afinním prostoru můžeme klasifikovat.

### D: Metrické
$$
\begin{equation}
\begin{aligned}
\bar{\nabla}g=0
\end{aligned}
\end{equation}
$$
Příkladem: [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace)

### D: Kontorze
Hyperonymum torze. Definujeme jako rozdíl dvou [CD](Kovariantn%C3%AD%20Derivace#D%20Kovariantn%C3%AD%20derivace%20(CD)) typicky od [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace). Kromě [Torze](Kovariantn%C3%AD%20Derivace#D%20Torze) zahrnuje i nekompatibilitu s metrikou; tj. obě vlastnosti, které má [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace) nulové.
$$
\begin{equation}
\begin{aligned}
\bar{\nabla}=\nabla+\frac{1}{2}\mathbb{C}
\end{aligned}
\end{equation}
$$
Oproti [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace):
$$
\begin{equation}
\begin{aligned}
\bar{\nabla}_{\underline{ a }}g_{\underline{ bc }}=:-2\mathbb{C}_{\underline{ a }(\underline{ bc })}
\end{aligned}
\end{equation}
$$

_Konvence:_ $\mathbb{C}={{C_{\underline{ a }}}^{\underline{ m }}}{}_{\underline{ b }}$.
_Speciálně:_ Pro [metrické](#D%20Metrick%C3%A9) derivace, tedy získáme informaci jen o torzi:
$$
\begin{equation}
\begin{aligned}
C_{\underline{ abc }}=T_{\underline{ abc }}-T_{\underline{ bca }}-T_{\underline{ cba }}
\end{aligned}
\end{equation}
$$

### D: LC-Geodetičnost
Třída [kovariantních derivací](Kovariantn%C3%AD%20Derivace#D%20Kovariantn%C3%AD%20derivace%20(CD)), které mají stejné geodetiky jako [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace).
$$
\begin{equation}
\begin{aligned}
\hat{\nabla}=\nabla+\hat{\Gamma}
\end{aligned}
\end{equation}
$$
Stejné geodetiky z [ekvivalence geodetik](Geodetiky#V%C4%9Bta%20Ekvivalence%20geodetik%20pro%20(CD)) znamená, totální antisymetrii indexů : $\hat{\Gamma}_{(\underline{ ab })}^{\underline{ m }}=0$.
Nebo ekvivalentně pro $\hat{C}_{\underline{ abc }}=\hat{C}_{\underline{ a }[\underline{ bc }]}$.

### Příklady:
Speciálně pro [geodetické](#D%20LC-Geodeti%C4%8Dnost) a [metrické](#D%20Metrick%C3%A9) derivace, dostáváme postupně podmínky na kontorzi
$$
\begin{equation}
\begin{aligned}
\hat{C}_{\underline{ abc }}&=\hat{C}_{\underline{ a }[\underline{ bc }]}\\
\hat{C}_{\underline{ abc }}&=\hat{C}_{[\underline{ a }\underline{ |b|c }]}\\
\iff\\
\hat{C}_{\underline{ abc }}&=\hat{C}_{[\underline{ a }\underline{ bc }]}=\hat{T}_{\underline{ abc }}
\end{aligned}
\end{equation}
$$
Tedy [kontorze](#D%20Kontorze) je přímo [torze](Kovariantn%C3%AD%20Derivace#D%20Torze).

### Afinní prostor derivací
Pro danou metriku
![[afinní prostor konexí.svg|800]]  
kde
- $\Gamma=\frac{1}{2}A+\frac{1}{2}S$,
- $\tau_{\underline{ abc }}:=-C_{[\underline{ abc }]}$,
- $\alpha_{\underline{ abc }}=\frac{4}{3}S_{\underline{ a }[\underline{ bc }]}$.
