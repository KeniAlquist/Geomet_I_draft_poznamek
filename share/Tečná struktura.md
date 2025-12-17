---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 4
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
# Tečná struktura
Existují dva obecně používané postupy, jak konstruovat tečný prostor: Zavedením tečných vektorů nebo jako prostor derivací na prostoru funkcí.
## Prostor směrů
Parametrizovaná křivka $z(\alpha)$, která prochází skrze bod $\mathscr{x}$, $z:\mathbb{R}\to M$ a $z(0)\equiv \mathscr{x}$.
Definujeme derivaci podél křivky $z$ v bodě $\mathscr{x}$ jako
$$
\begin{equation}
\begin{aligned}
\left[ \frac{d}{d\alpha}\right]\Bigg|_{\alpha=0} f\circ z  
\end{aligned}
\end{equation}
$$
Říkáme, že křivky $z_{1}$ a $z_{2}$ mají stejné směry a rychlosti:
$$
\begin{equation}
\begin{aligned}
\forall f\in\mathcal{F}M, \text{ platí }: \left[ \frac{d}{d\alpha}  \right]_{0} f\circ z_{1} =\left[ \frac{d}{d\alpha}  \right]_{0} f\circ z_{2} 
\end{aligned}
\end{equation}
$$

#### D: Směr
Je třída ekvivalence křivek stejného směru, zn. $\dot{z}=\left[ \frac{D}{d\alpha} \right]_{0}z$

#### D: Derivace ve směru
Pro směr $a$ a jakákoliv křivka $z$ reprezentující směr $a$ definujeme derivaci funkce $f$ ve směru $a$ jako:
$$
\begin{equation}
\begin{aligned}
\ a[f]=\left[ \frac{d}{d\alpha} \right]_{0} f\circ z
\end{aligned}
\end{equation}
$$

Pozn. pro $f=F(f_{1},\dots ,f_{k})$, kde $f_{l}\in \mathcal{F}M$ a $F:\mathbb{R}^{k}\to \mathbb{R}$ hladká:
$$
\begin{equation}
\begin{aligned}
a[f]=\sum_{l}\frac{ \partial F }{ \partial f_{l} }_{(f_{1},\dots ,f_{k})} ~a[f_{l}] 
\end{aligned}
\end{equation}
$$

Derivace ve směru můžeme povýšit na diferenciální operátor 1. řádu, neboť splňuje jeho definici.
## Prostor derivací
Nebo můžeme prvně definovat prostor derivací.
#### D: Prostor derivací na $\mathcal{F}M$ v bodě $\mathscr{x}$
$a$ je derivace na $\mathcal{F}M$ v bodě $\mathscr{x}$, pokud:
1) $a:\mathcal{F}M\to \mathbb{R}$
2) $a[rf]=ra[f]$
   $~a[f+g]=a[f]+a[g]$
3) $a[fg]_{\mathscr{x}}=a[f]g~|_{\mathscr{x}}+fa[g]|_{\mathscr{x}}$
Lineární zobrazení z funkcí do $\mathbb{R}$ splňující Leibnizovo pravidlo.

**Vlastnosti:** 
1) Lokalita je už zahrnuta.
* $f=0$ na okolí $\mathscr{x}\implies a[f]_{\mathscr{x}}=0$
* $f=C$ na okolí $\mathscr{x}\implies a[f]_{\mathscr{x}}=0$
* $f=g$ na okolí $\mathscr{x}\implies a[f]_{\mathscr{x}}=a[g]_{\mathscr{x}}$
neboť pro $\mathscr{x}\in U$, $f|_{U}=0$ a $\beta|_{\mathscr{x}}=1$ (support $U$), kdy je $f\beta=0$ všude. Pak derivace
$0=a[f\beta]_{\mathscr{x}}=fa[\beta]|_{\mathscr{x}}+\beta a[f]|_{\mathscr{x}}=a[f]$.
a pro $f=C \overset{\text{BÚNO}}=1$: $a[1]=a[1^{2}]=2a[1] \implies a[1]=0$.

2) Derivace složené funkce 
$$\begin{equation}
\begin{aligned}
a[f]=\sum_{l}\frac{ \partial F }{ \partial f_{l} }_{(f_{1},\dots ,f_{k})} ~a[f_{l}] 
\end{aligned}
\end{equation}$$
už plyne z definice.

#### D: Tečný prostor $\mathbf{T}_{\mathscr{x}}M$
$\mathbf{T}_{\mathscr{x}}M$ je prostor všech derivací $\mathcal{F}M$ v $\mathscr{x}$. Je inherentně lineární struktura z definice $a[~]$.

#### D: Tečný bandl $\mathbf{T}_{}M$
$\mathbf{T}_{}M=\bigcup_{\mathscr{x\in}M}\mathbf{T}_{\mathscr{x}}M$. Jakožto soubor tečný prostorů ve všech bodech $M$.

Pokud byly mapy kompatibilní na $M$ tak budou i na $\mathbf{T}_{}M$.
Tvoří diferencovatelnou varietu, protože lze opět popsat souřadnicově a mapy jsou kompatibilní.
Souřadnice $x^{i}$ akorát rozšíříme o $a[x^{i}]=a^{i}$ na $x_{*}:=[x^{i},a^{i}]$.
Neboli pro mapu $(U,X)$ na $M$ dostáváme mapu $(\mathbf{T}_{}U,x_{*})$ na $\mathbf{T}_{}M$. 

#### D: Kotečný prostor $\mathbf{T}_{\mathscr{x}}^*M$
$\mathbf{T}_{\mathscr{x}}^*M$ definujeme jako duální prostor k $\mathbf{T}_{\mathscr{x}}M$, tj. pro
$$
\begin{equation}
\begin{aligned}
\alpha \in \mathbf{T}_{\mathscr{x}}^*M : \alpha[a]\equiv a[\alpha]\equiv \langle\alpha;a\rangle \equiv \alpha \cdot a
\end{aligned}
\end{equation}
$$
Hovoříme o duální bázi prostoru jako bázi $e^{i}$ generující $\mathbf{T}_{\mathscr{x}}^*M$, pokud pro bázi $e_{i}$ generující $\mathbf{T}_{\mathscr{x}}M$:
$e^{i}\cdot e_{j}=\delta^{i}_{j}$.



**Vlastnosti**
- ${~d}f$: 
Užitečný příklad prvku kotečného prostoru je *gradient*. 1-forma $\mathrm{d}f$  jakožto $a[f]=a\cdot {d}f$.
1) ${d}(f+g)={d}f+{d}g$
2) ${d}(rf)=r{~d}f$
3) ${d}(fg)={~d}f~ g+f{~d}g$
$f=F(f_{1},\dots,f_{k})$
4) ${~d}f=\sum_{l}\partial_{f_{l}}F(\dots){~d}f_{l}$

- souřadnice
báze v $\mathbf{T}_{}M: \partial_{x^{i}}\equiv \frac{ \partial }{ \partial x^{i} }$ 
báze v $\mathbf{T}_{}^*M:{~d}x^{i}$
pak souřadnice v dané bázi
1) $a^{i}=a\cdot dx^{i}$
2) $\alpha_{j}=\alpha \cdot \partial_{x^{j}}$

- Transformace souřadnic
1) ${~d}x^{k'} = \frac{ \partial x^{k'} }{ \partial x^{l} }{~d}x^{l}$
2) $\partial_{x^{k'}}=\frac{ \partial x^{l} }{ \partial x^{k'} }\partial_{x^{l}}$.

- Poznámky:
1) Varietu lze obecně rozšiřovat o další struktury (VP) tzv. fibrované prostory.
2) Jiné podobné algebraické struktury jsou Okruhy, kde se vypouští požadavek existence inverze mimo 0. ”tečnou strukturou” jsou moduly.