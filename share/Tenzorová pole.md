---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 7
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

# Tenzorová pole
### D: Tenzorové pole typu $(K,L)$
$\mathcal{T}^{K}_{L}M$ je prostor tečných bandlů. Řez tímto prostorem  $\mathbf{T}^{K}_{L}M$
$$
\begin{equation}
\begin{aligned}
\mathcal{T}^{K}_{L}M\ni A:M&\to \mathbf{T}^{K}_{L}M\\
\mathscr{x}&\mapsto A(\mathscr{x})\in {\mathbf{T}_{\mathscr{x}}}^{K}_{L}M
\end{aligned}
\end{equation}
$$
Je to speciální tenzorový součin $\mathcal{F}M$ modulů. $f(\mathscr{x})A^{\underline{ m }\dots}_{\underline{ n }\dots}(\mathscr{x})$ je lokální.

### D: Tenzorové pole jako univerzální $\mathcal{F}M$-lineární zobrazení
$$
\begin{equation}
\begin{aligned}
l: \mathcal{T}_{L_{1}}^{K_{1}}M\times\mathcal{T}_{L_{2}}^{K_{2}}M\times\cdots\to \mathcal{T}_{L}^{K}M
\end{aligned}
\end{equation}
$$
a je ultra-lokální
$$
\begin{equation}
\begin{aligned}
l(\dots,f\mathbf{A},\dots)=fl(\dots,A, \dots)
\end{aligned}
\end{equation}
$$

Toto zobrazení, lze reprezentovat tenzorovým polem
$$
\begin{equation}
\begin{aligned}
l^{\underline{ A }}(T_{1},T_{2},\dots)(\mathscr{x})
=L^{\underline{ A }}_{\underline{ A }_{1}\underline{ A }_{2}}(\mathscr{x})
~ T_{1}^{\underline{ A }_{1}}(\mathscr{x})
~ T_{2}^{\underline{ A }_{2}}(\mathscr{x}) \cdots
\end{aligned}
\end{equation}
$$
(jelikož lze reprezentovat tenzorovým polem, je vlastnost zděděná z lineárních prostorů)

### D: $\mathbf{D}$ Tenzorová derivace
Zobrazení $\mathbf{D}:\mathcal{T}_{L}^{K}M\to \mathcal{T}_{L}^{K}M$ splňunjící
- $\mathbf{D}(A+B)=\mathbf{D}A+\mathbf{D}A$
- $\mathbf{D}(AB)=(\mathbf{D}A)B+A(\mathbf{D}B)$
- $\mathbf{D}(\text{C}A)=\text{C}\mathbf{D}A$
(není jednoznačná)

Zobecněná tenzorová derivace
$\mathbf{D}:\mathcal{T}_{L}^{K}M\to\mathcal{T}_{L+Q}^{K+P}M$
$$
\begin{equation}
\begin{aligned}
\mathbf{D}_{\underline{ q }\dots}^{\underline{ p }\dots} A_{\underline{ b }\dots}^{\underline{ a }\dots}
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\mathbf{D}_{T}=T^{\underline{ q }\dots}_{\underline{ p }\dots} ~\mathbf{D}^{\underline{ p }\dots}_{\underline{ q }\dots}
\end{aligned}
\end{equation}
$$

### Lemma: Restrikce $\mathbf{D}$ na $\mathcal{F}M$
Restrikce derivace D na modul $\mathcal{F}M$ je dána $\mathbf{D}f=a[f]=a\cdot \mathrm{d}f$

### Věta: $\mathbf{D_{1},D_{2}}$ shodnost na $\mathcal{F}M$ a $\mathcal{T}_{}M$
Pokud jsou derivace $D_{1}$ a $D_{2}$ stejné na $\mathcal{F}M$ a $\mathcal{T}M$, pak musí být stejné i pro tenzory.

### D: Pseudoderivace $\mathbf{M}$
- $\mathbf{M}$ je derivace
- $\mathbf{M}f=0;\,\forall f\in\mathcal{F}M$
	(ultralokální, neboť: $\mathbf{M}(fA)=\cancelto{ 0 }{ (\mathbf{M}f) }+f\mathbf{M}A$)

Poznámka.: Pseudoderivace jsou později nástrojem měření nekomutativity derivací.
### lemma: $\mathbf{M}$ je dána akcí na $\mathcal{T}_{}M$
Pseudoderivace $\mathbf{M}$. Pak $\mathbf{M}a^{\underline{ m }}=M^{\underline{ m }}_{\underline{ a }}a^{\underline{ a }}$ díky $\mathcal{F}M$-linearitě lze vyjádřit tenzorem. Víme, že
$$
\begin{equation}
\begin{aligned}
0 = \mathbf{M}(\alpha_{\underline{ m }}a^{\underline{ m }})
= (\mathbf{M}\alpha_{\underline{ n }})a^{\underline{ n }}
+ \alpha_{\underline{ n }}(\mathbf{M}a^{\underline{ n }})
= (\mathbf{M}\alpha_{\underline{ n }})a^{\underline{ n }}
+ \alpha_{\underline{ n }}
~ M^{\underline{ n }}_{\underline{ m }}a^{\underline{ m }}
\end{aligned}
\end{equation}
$$
tedy na kovektorech už nutně působí jako:
$$
\begin{equation}
\begin{aligned}
\mathbf{M}\alpha_{\underline{ n }}
= - M^{\underline{ m }}_{\underline{ n }}\alpha_{\underline{ m }}
\end{aligned}
\end{equation}
$$
Na tenzor tedy působí jako
$$
\begin{equation}
\begin{aligned}
\mathbf{M}T^{\underline{ m }\underline{ n }}_{\underline{ k }\underline{ l }}
= M^{\underline{ m }}_{\underline{ p }} 
~T^{\underline{ p }\underline{ n }\dots}_{\underline{ k }\underline{ l }\dots} 
+ \dots
-M^{\underline{ q }}_{\underline{ k }} 
~T^{\underline{ m }\underline{ n }\dots}_{\underline{ q }\underline{ l }\dots}
- \cdots
\end{aligned}
\end{equation}
$$

### Věta: $\mathbf{D_{1},D_{2}}$ shodnost na $\mathcal{F}M$
implikuje, že jsou si derivace rovny až a pseudoderivaci $\mathbf{D_{2}}-\mathbf{D_{1}}=\mathbf{M}$


### D: Transformace $\mathbf{G}$
$\mathcal{F}M$-lineární nedegenerovanou transformaci na $\mathcal{T}_{}M$.
$$
\begin{equation}
\begin{aligned}
(\mathbf{G}a)^{\underline{ m }}=G^{\underline{ m }}_{\underline{ n }}a^{\underline{ n }}
\end{aligned}
\end{equation}
$$
na tenzorech tedy:
$$
\begin{equation}
\begin{aligned}
(\mathbf{G}T)^{\underline{ a }\underline{ b }}_{\underline{ c }\underline{ d }}
= G^{\underline{ a }}_{\underline{ m }}
~ G^{\underline{ b }}_{\underline{ n }}\dots
~ {G^{-1}}^{\underline{ k }}_{\underline{ c }}
~ {G^{-1}}^{\underline{ l }}_{\underline{ d }}
~ T^{\underline{ m }\underline{ n }}_{\underline{ k }\underline{ l }}
\end{aligned}
\end{equation}
$$
- $\mathbf{G}$ komutuje s $\otimes$: $\mathbf{G}(a^{\underline{ n }}b^{\underline{ m }})=(\mathbf{G}a^{\underline{ n }})(\mathbf{G}b^{\underline{ m }})$
- $\mathbf{G}$ komutuje s $\text{C}$ pak $a^{\underline{ m }}\alpha_{\underline{ m }}\overset{!}=\mathbf{G}(a^{\underline{ m }}\alpha_{\underline{ m }})=(G^{\underline{ m }}_{\underline{ l }}a^{\underline{ l }})(\tilde{G}^{\underline{ k }}_{\underline{ m }}\alpha_{\underline{ k }})\implies \tilde{G}=G^{-1}$
infinitesimální rozvoj transformace:
$$
\begin{equation}
\begin{aligned}
(\mathbf{G}_{\varepsilon}T)^{\underline{ a }\underline{ b }\dots}_{\underline{ c }\underline{ d }\dots}
= T^{\underline{ ab }\dots}_{\underline{ cd }\dots}+ \varepsilon\left[M^{\underline{ a }}_{\underline{ m }}~T^{\underline{ m }\underline{ b }\dots}_{\underline{ cd }\dots}+ M^{\underline{ b }}_{\underline{ m }}
  ~ T^{\underline{ a }\underline{ m }\dots}_{\underline{ cd }\dots}+\dots- M^{\underline{ m }}_{\underline{ d }}
  ~ T^{\underline{ a }\underline{ b }\dots}_{\underline{ cm }\dots}- M^{\underline{ m }}_{\underline{ c }}
  ~ T^{\underline{ a }\underline{ b }\dots}_{\underline{ md }\dots}-\dots\right]
\end{aligned}
\end{equation}
$$
