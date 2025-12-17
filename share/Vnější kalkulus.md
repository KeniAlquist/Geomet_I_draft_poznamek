---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 8
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

# Vnější Kalkulus
Značení, na varietě $M$ označíme $\Lambda^{p}M=\mathbf{T}^{0}_{[p]}M$, (0-vektorových, p-antisymetrizovaných kovektorových indexů) Na tenzorových polích pak $\mathcal{A}^{p}M=\mathcal{T}_{[p]}^{0}M$.
Jako _Vnější algebru_ označíme $\Lambda M=\oplus^{d}_{p=0}\Lambda^{p}M$, respektive pro tenzorová pole $\mathcal{A}_{}M=\oplus^{d}_{p=0}\mathcal{A}^{p}M$.

### D: Vnější součin
Pro k-forem $\alpha^{j}\in V_{[P_{j}]}$, $\sum_{j}^{k}P_{j}=P$ je jejich vnější součin
$$
\begin{equation}
\begin{aligned}
\alpha^{1}\wedge \alpha^{2} \wedge\dots \wedge\alpha^{k}
= \frac{P!}{P_{1}!\cdots P_{k}!} \mathcal{A}_{}(\alpha^{1}\alpha^{2}\cdots\alpha^{k}) 
\end{aligned}
\end{equation}
$$
pomocí abstraktních indexů:
$$
\begin{equation}
\begin{aligned}
(\alpha^{1}\wedge \alpha^{2} \wedge\dots \wedge\alpha^{k})_{\underline{ a }_{1}\dots \underline{ a }_{P}}
= \frac{P!}{P_{1}!\cdots P_{k}!} \alpha^{1}_{\big[\underline{ a }_{1}\dots \underline{ a }_{P_{1}}}\alpha^{2}_{\underline{ a }_{P_{1}+1}\dots \underline{ a }_{P_{2}}}\cdots\alpha^{k}_{\underline{ a }_{P_{2}+1}\dots \underline{ a }_{P}\big]} 
\end{aligned}
\end{equation}
$$
(vzhledem k tomu, že $\alpha$ už májí samy o sobě abstraktní indexy antisymetrizované, musíme kompenzovat pomocí permutací $P_{j}!$ z duplicity)
__Vlastnosti:__
- Asociativita
$$
\begin{equation}
\begin{aligned}
(\alpha \wedge \beta)\wedge\gamma=\alpha \wedge(\beta \wedge\gamma)
\end{aligned}
\end{equation}
$$
- skew-komutativita
$$
\begin{equation}
\begin{aligned}
\alpha \wedge \beta = (-1)^{pq} \beta \wedge\alpha
\end{aligned}
\end{equation}
$$

### D: Kontrakce
Zobrazení $\bullet: V_{[K]}\times V^{[K]}\to \mathbb{R}$
$$
\begin{equation}
\begin{aligned}
\omega \bullet w 
&= \frac{1}{K!}\text{C}\omega w\\
&= \frac{1}{K!}\omega_{\underline{ a }_{1}\dots\underline{ a }_{K}}w^{\underline{ a }_{1}\dots\underline{ a }_{K}}
\end{aligned}
\end{equation}
$$

Identita na antisymetrických tenzorech, když přes bullet, tak se musím postarat o duplicity:
$^{[K]}\delta$ ... $\mathcal{A}_{}A=K!~^{[K]}\delta \bullet A$.
### D: Insert
Zobrazení $\iota_{u}:V_{[K]}\to V_{[K-1]},\,u\in V$.
$$
\begin{equation}
\begin{aligned}
\iota_{u}=u\cdot \omega=u\bullet\omega\\
(\iota_{u}\omega)_{\underline{ a }_{2}\dots \underline{ a }_{P}}=u^{\underline{ n }}\omega_{\underline{ n }\underline{ a }_{2}\dots \underline{ P }}
\end{aligned}
\end{equation}
$$
__Vlastnosti:__
$$
\begin{equation}
\begin{aligned}
&u \bullet (\alpha \wedge\beta)
= (u\bullet\alpha)\wedge \beta+(-1)^{P}\alpha \wedge(u\bullet\beta )\\
&\alpha\in V_{[P]}
\end{aligned}
\end{equation}
$$
abstraktní index $u$ se musí prokomutovat přes formy $\alpha$ k $\beta$.
$\iota_{a}(\omega+\sigma)=\iota_{a}\omega+\iota_{a}\sigma$
$\iota_{a}(\omega \wedge\sigma)=(\iota_{a}\omega)\wedge\sigma+(-1)^{P}\omega \wedge(\iota_{a}\sigma)$
$\iota_{a}f=0$
$\iota_{a}\alpha=a\cdot\alpha$
$\iota_{a}\omega=a\bullet\omega$

### Souřadnicová báze na prostoru antisymetrických tenzorů
$V\,|\,e^{\underline{ a }}_{k}$   a $V^{*}\,|\,e^{k}_{\underline{ a }}$    pak $e^{\underline{ m }}_{k}e^{l}_{\underline{ m }}=\delta^{l}_{k}$.
Obecně $V^{[P]}$ generující množina $e_{k_{1}\dots k_{p}}=e_{k_{1}}\wedge\dots \wedge e_{k_{p}}\,(=p!\mathcal{A}_{}(e_{k_{1}}\dots e_{k_{p}}))$ , ale takto je příliš velká (objevují je lineárně závislé, když bych prohazoval $k_{j}$ a $k_{l}$) $\to$ požaduji uspořádání $k_{1}<\dots<k_{p}$.
Duální bázi už můžu doplnit pomocí bullet, díky uspořádání:
$$
\begin{equation}
\begin{aligned}
e_{k_{1}\dots k_{p}}\bullet e^{l_{1}\dots l_{p}}=\delta^{l_{1}}_{k_{1}}\cdots\delta^{l_{p}}_{k_{p}}
\end{aligned}
\end{equation}
$$


__Antisymetrické tenzory v komponentách:__
$\omega\in V_{[p]}$
$$
\begin{equation}
\begin{aligned}
\omega
&= \omega_{a_{1}\dots a_{p}}e^{a_{1}}\otimes \cdots \otimes e^{ a_{p} }\\
&=\omega_{a_{1}\dots a_{p}}\mathcal{A}_{}(e^{a_{1}}\dots e^{a_{p}})\\
&= \sum_{a_{1}<\dots<a_{p}}\omega_{a_{1}\dots a_{p}}~p!\mathcal{A}_{}(e^{a_{1}}\dots e^{a_{p}})\\
&=\sum_{a_{1}<\dots <a_{p}}\omega_{a_{1}\dots a_{p}} ~ e^{a_{1}\dots a_{p}}
\end{aligned}
\end{equation}
$$
(implicitně předpokládáme, že v tenzorovém součinu jsou indexy antisymetrizované)
### D: Vnější derivace (už nutně na formách)
Zobrazení $\mathrm{~d}:\mathcal{A}^{p}M\to\mathcal{A}^{p+1}M$, splňující:
- $\mathrm{~d}(\omega+r\sigma)=\mathrm{~d}\omega+r\mathrm{~d}\sigma,\,r\in \mathbb{R}$
- $\mathrm{~d}(\omega \wedge\sigma)=\mathrm{~d}\omega \wedge\sigma+(-1)^{p}~\omega \wedge \mathrm{d}\sigma$
- $\mathrm{~d}f=df,\,\in \mathcal{F}M,$ tj. $\mathcal{A}^{0}_{}M$
- $\mathrm{~d}\mathrm{d}\omega=0$

Tímto je tato derivace určena jednoznačně.
DK:
$$
\begin{equation}
\begin{aligned}
\mathrm{~d}\omega
&=\mathrm{~d}\left( \sum_{a_{1}<\dots <a_{p}} \omega_{a_{1}\dots a_{p}}\mathrm{~d}x^{a_{1}}\wedge\dots \wedge\mathrm{~d}x^{a_{p}} \right)\\
&= \sum_{a_{1}<\dots <a_{p}} \mathrm{~d}\omega_{a_{1}\dots a_{p}}\wedge\mathrm{~d}x^{a_{1}}\wedge\dots \wedge\mathrm{~d}x^{a_{p}}+\cancelto{ 0 }{ \mathrm{~d}\mathrm{d}x }\\
&= \sum_{a_{1}<\dots <a_{p}} \omega_{a_{1}\dots a_{p}a}\mathrm{~d}x^{a}\wedge\mathrm{~d}x^{a_{1}}\wedge\dots \wedge\mathrm{~d}x^{a_{p}}\\
&= \frac{1}{p!} \omega_{a_{1}\dots a_{p}a_{0}}\mathrm{~d}x^{a_{0}}\wedge\mathrm{~d}x^{a_{1}}\wedge\dots \wedge\mathrm{~d}x^{a_{p}}\\
&= \sum_{a_{0}<\dots <a_{p}} \frac{1}{p!}(p+1)!~\omega_{a_{1}\dots a_{p}a_{0}}\mathrm{~d}x^{a_{0}}\wedge\mathrm{~d}x^{a_{1}}\wedge\dots \wedge\mathrm{~d}x^{a_{p}}\\
&= (p+1) \omega_{a_{1}\dots a_{p}a_{0}}
\end{aligned}
\end{equation}
$$
nezávisle na souřadnicích. Můžeme se podívat na přechod mezi souřadnicovou bází.
$$
\begin{equation}
\begin{aligned}
\omega_{a_{1}'\dots a_{p}'}=X^{n_{1}}_{a_{1}'}\cdots X^{n_{p}}_{a_{p}'} ~\omega_{n_{1}\dots n_{p}}
\end{aligned}
\end{equation}
$$
pak se
$$
\begin{equation}
\begin{aligned}
(\mathrm{d}\omega)_{a_{0}'\dots a_{1}'}
&=(p+1)\omega_{a_{1}'\dots a_{p}'a_{0}'}\\
&= (p+1)(\omega_{n_{1}\dots n_{p}}~X^{n_{1}}_{a_{1}'}\cdots X^{n_{p}}_{a_{p}'})_{a_{0}'}\\
&\text{Všem objektům v závorce se přiřadí index, ale }X_{a_{1}'a_{0}'} \text{ je sym}\\
&=(p+1)(\omega_{n_{1}\dots n_{p}a_{0'}}~X^{n_{1}}_{a_{1}'}\cdots X^{n_{p}}_{a_{p}'})\\
&=(p+1)(\omega_{n_{1}\dots n_{p}n_{0}}~X^{n_{0}}_{a_{0}'}X^{n_{1}}_{a_{1}'}\cdots X^{n_{p}}_{a_{p}'})\\
&=(\mathrm{d}\omega)_{n_{0}\dots n_{p}}~X^{n_{0}}_{a_{0}'}\cdots X^{n_{p}}_{a_{p}'}
\end{aligned}
\end{equation}
$$
a transformuje se správně.

### Cartanovy vzorce
Pro $\gamma$ $1$-formu platí
$$
\begin{equation}
\begin{aligned}
a\cdot \mathrm{d}\gamma \cdot b= a\cdot \mathrm{d}(\gamma \cdot b)-\mathrm{d}(a\cdot\gamma)\cdot b - [a;b]\cdot\gamma
\end{aligned}
\end{equation}
$$
