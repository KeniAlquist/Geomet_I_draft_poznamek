---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 10
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

# Indukovaná zobrazení
Jako zobrazení akcí při mapovaní mezi varietami obecných dimenzí.

$\phi:M\to N$, 
takové, že zobrazení je hladké na $\mathbb{R}^{M}\to \mathbb{R}^{N}$.

### D: Pull-back funkce
$\phi^{*}:\mathcal{F}N\to \mathcal{F}M$
$$
\begin{equation}
\begin{aligned}
(\phi^{*}f)_{(x)}=f(\phi x)
\end{aligned}
\end{equation}
$$
### D: Push-forward vektoru
$\phi_{*}:\mathbf{T}_{\mathscr{x}}M\to \mathbf{T}_{\phi \mathscr{x}}N$
![[Drawing 2025-10-24 11.57.16.excalidraw.svg|400]]
$$
\begin{equation}
\begin{aligned}
\phi_{*}\frac{ D z }{ \partial t }=\frac{ D \phi z }{ \partial t }  
\end{aligned}
\end{equation}
$$

### D: Pull-back kovektorů
$\phi^{*}:\mathbf{T}^{*}_{\phi \mathscr{x}}N\to \mathbf{T}^{*}_{\mathscr{x}}M$
![[pull-back.svg|400]]
Definované z akce na vektorech, tj. využitím duality.

__Příklady k rozmyšlení:__
- $(\phi_{*}a)|_{\phi \mathscr{x}}(f)=a|_{\mathscr{x}}(\phi^{*}f)$
- $\phi^{*}(\mathrm{d}f)=d(\phi^{*}f)$

### Push-forward je lineární operace
$\phi_{*}(a+rb)=\phi_{*}a+r\phi_{*}b,\,r\in \mathbb{R}$.
$\phi^{*}(\alpha+r\beta)=\phi^{*}\alpha+r\phi^{*}\beta$
A tedy ji lze reprezentovat nějakým tenzorem (rozlišujeme abstraktní indexy na varietách $\sim$ ):
$$
\begin{equation}
\begin{aligned}
(\phi_{*}a)^{\underline{ \tilde{m }}}=(D\phi)^{\underline{ \tilde{m} }}_{\underline{ n }}a^{\underline{ n }}\\
(\phi^{*}\alpha)_{\underline{ \tilde{n }}}=(D\phi)^{\underline{ \tilde{m} }}_{\underline{ n }}\alpha_{\underline{ \underline{ m } }}
\end{aligned}
\end{equation}
$$
Tento $D\phi$ tenzor nazýváme diferenciál zobrazení. 
!! Nelze obecně dělat push-forward a pull-back na obecných tenzorech, nejde to kombinovat obecně.
Lze to pro diffeomorfizmy !!

### D: Diffeomorfizmy
1-1 značné zobrazení. $\phi\,\phi^{-1}$.
Pro ně můžeme definovat push-forward pro kovektory jako inverzi pull-backu.
$\phi_{*}\alpha:={\phi^{-1}}^*\alpha$, kde $\alpha \in \mathbf{T}^{*}M$

__Speciálně:__
1) v 1D, kdy  $M\to \mathbb{R}$, $f:M\to \mathbb{R}$.
$\mathrm{D}f=\mathrm{d}f\frac{ \partial  }{ \partial \tau }$, je pak ekvivalentní gradientu.
2) křivka, kdy $\mathbb{R}\to N$
$\mathrm{D}z=\mathrm{d}\tau \frac{ D z }{ \partial \tau }$, je pak ekvivalentní tečnému vektoru ke křivce.

Tenzor můžeme vyjádřit v souřadnicích po zúžení s duální bází
$$
\begin{equation}
\begin{aligned}
\mathrm{D}\phi &\in \mathbf{T}^{*}M\otimes \mathbf{T}N\\
&\,x \in M\,\,y\in N
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\mathrm{D}\phi=\frac{ \partial \bar{\phi}^{\tilde{k}} }{ \partial x^{l} } \mathrm{~d}x^{l}\frac{ \partial }{ \partial y_{\tilde{k}} } ,
\end{aligned}
\end{equation}
$$
kde je $\bar{\phi}$ známé značení pro vyjádření na $\mathbb{R}$ prostorech. $\bar{\phi}=y\circ\phi\circ x^{-1}$.
$$
\begin{equation}
\begin{aligned}
(\mathrm{D}\phi)^{\tilde{k}}_{l}
&=\mathrm{d}y^{\tilde{k}}\cdot \mathrm{D}\phi\cdot \frac{ \partial  }{ \partial x^{l} } \\
& \overset{\text{def}}=\mathrm{d}y^{\tilde{k}}\cdot \phi_{*}\frac{ \partial  }{ \partial x^{l} } \\
& \overset{\text{def}}=\left[\phi_{*} \frac{ \partial  }{ \partial x^{l} }\right]y^{\tilde{k}} 
& \overset{\text{neformálně}}=\frac{ \partial y^{\tilde{k}} }{ \partial x^{l} } =\frac{ \partial \bar{\phi}^{\tilde{k}} }{ \partial x^{l} } 
\end{aligned}
\end{equation}
$$
neboli k získání souřadnic tenzoru, děláme parciální derivaci $y^{\tilde{k}}=\bar{\phi}^{\tilde{k}}(x^{1},\dots,x^{m})$.

Takto jsme se zatím bavili pouze o lokálně, ale jelikož to můžeme udělat v každém bodě, můžeme to zobecnit na pole.
### Indukovaná zobrazení na polích
(reminder: funkce $\phi^{*}: \mathcal{F}N\to \mathcal{F}M$ )
Formy          $\phi^{*}:\mathcal{T}^{0}_{p}N\to\mathcal{T}^{0}_{p}M$
$$
\begin{equation}
\begin{aligned}
(\phi^{*}\tilde{\omega})_{(\mathscr{x})}:=\phi^{*}(\tilde{\omega}_{(\phi \mathscr{x})})
\end{aligned}
\end{equation}
$$

Lemma pro funkce s push-forwardem   $\phi_{*} \pu{ d }f = \pu{ d }\phi^{*}f$ .   To ale neplatí pro push-forward obecně na tenzorech, protože push-forwardem neumím působit na funkce (souřadnice)!!! Opět zachrání diffeomorfizmy: $\phi,\phi^{-1}$.
$$
\begin{equation}
\begin{aligned}
(\phi_{*}A)_{(\mathscr{\phi x})}:=\phi_{*}(A_{(\mathscr{x})})
\end{aligned}
\end{equation}
$$
ale na to jsem využil tu inverzi.

__Vlastnosti push-forwardu pro pole:__
- $\phi_{*}(A+rB)=\phi_{*}A+r\phi_{*}B$
- $\phi_{*}(A\otimes B)=\phi_{*}A\otimes \phi_{*}B$
- $\phi_{*}\pu{ C }A=\pu{ C }\phi_{*}A$

__Příklady na rozmyšlení:__
- $a[\phi^{*}\tilde{f}]= \phi^{*}((\phi_{*}a)[\tilde{f}])$
pro diffeomorfizmy ekvivalentní
- $\phi_{*}(a[\phi^{*}\tilde{f}])=(\phi_{*}a)[\tilde{f}]$
- $\phi_{*}(a[f])=(\phi_{*}a)[\phi_{*}f]$
(páč $(\phi_{*}a)[\phi^{-1*}f]=(\phi_{*}a)[\phi_{*}f]$)
- $\phi_{{*}}[a;b]=[\phi_{*}a;\phi_{*}b]$ lze takhle push-forwardit lie závorku
Dk:
$$
\begin{equation}
\begin{aligned}
\phi^{*}\left(\big[\phi_{*}[a;b]\big]\tilde{f}\right)&=[a;b]\phi^{*}\tilde{f}=a[b[\phi^{*}\tilde{f}]]-b[a[\phi^{*}\tilde{f}]]=a\left[\phi^{*}\left((\phi_{*}b)[\tilde{f}]\right) \right]-b\left[\phi^{*}\left((\phi_{*}a)[\tilde{f}]\right) \right]\\
&=\phi^{*}\left(\left(\phi_{*}a\right)\left[(\phi_{*}b)\tilde{f}\right]\right)-\phi^{*}\left(\left(\phi_{*}b\right)\left[(\phi_{*}a)\tilde{f}\right]\right)\\
&=\phi^{*}\left([\phi_{*}a;\phi_{*}b]\tilde{f}\right)
\end{aligned}
\end{equation}
$$
