---
tags:
  - Magistr/Geomet/Geomet_I/Geomet_I
Téma: 23
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

# Integrální věty
Připomeňme některé pojmy z kapitoly [Podvariety a Tečné distribuce](Podvarieta%20a%20Te%C4%8Dn%C3%A9%20Distribuce). [Tečnými vektory](Podvarieta%20a%20Tečné%20Distribuce#Tečné%20vektory%20k%20N) rozumíme vektory ležící na podvarietě $N\subset M$. [Normálovými kovektory](Podvarieta%20a%20Tečné%20Distribuce#Normálové%20kovektory) podvariety $N$ jsou pak takové, které tečné vektory anihilují. Normálové kovektory z $\mathbf{N}^{*}_{\mathscr{x}}N$ nejsou nutně jednoznačně určena pro hypervarietě $\mathbf{T}^{*}_{\mathscr{x}}M$! 
Užitečné je zavést [přizpůsobené souřadnice](Podvarieta%20a%20Tečné%20Distribuce#Tečné%20struktury%20v%20souřadnicích) podvarietě $N$ na varietě $M$. 
$x^{u}=\text{konst.}$ definují podvarietu $N$, $u=1\dots \hat{n}$. Zbylé souřadnice $x^{i}$ na $N$, $i=\hat{n}+1,\dots \hat{n}+n$.
V nichž můžu vyjadřovat známé objekty.

[Tenzoru orientace](Integrovateln%C3%A9%20hustoty#D%20Tenzor%20orientace) na varietě $M$, resp. $N$ 
$\tilde{\varepsilon}_{M}=(\mathrm{d}^{m}x)^{-1}~\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{m}$    a inverzní  $\tilde{\varepsilon}^{-1}_{M}=(\mathrm{d}^{m}x)~\partial_{x^{1}}\wedge\dots \wedge \partial_{x^{m}}$
$\tilde{\varepsilon}_{N}=(\mathrm{d}^{n}x)^{-1}~\mathrm{d}x^{\hat{n}+1}\wedge\dots \wedge \mathrm{d}x^{\hat{n}+n}$    a inverzní  $\tilde{\varepsilon}^{-1}_{N}=(\mathrm{d}^{n}x)~\partial_{x^{\hat{n}+1}}\wedge\dots \wedge \partial_{x^{m}}$

Tečné vektory $N$ jsou pak lineární obal $\partial_{x^{\hat{n}+1}},\dots , \partial_{x^{m}}$.
Normálové formy $N$ jsou pak lineární obal $\mathrm{d}x^{1},\dots , \mathrm{d}x^{m}$. Je zřejmé, že se navzájem anihilují.

Kotečnou strukturu forem na $N$, jsme zjistili, že lze nalézt [restrikcí formy na podvarietu](Podvarieta%20a%20Tečné%20Distribuce#Restrikce%20formy): $\omega \mapsto \omega|_{N}\in \mathbf{T}^{*}_{}N$. To samé lze provést přirozeně i pro **TAS formy**. Ty si také vyjádříme v souřadnicích, tím že si necháme jen ty souřadnice, které nejsou normálové ($\hat{n}<$):
$$
\begin{equation}
\begin{aligned}
\omega &=~\sum_{k_{1}<\dots<k_{p}}~~\omega_{k_{1}\dots k_{p}}~\mathrm{d}x^{k_{1}}\wedge\dots \wedge \mathrm{d}x^{k_{p}}~\in\Lambda^{p}M\\
&\downarrow\\
\omega|_{N}&=\sum_{\hat{n}<k_{1}<\dots<k_{p}}\omega_{k_{1}\dots k_{p}}~\mathrm{d}x^{k_{1}}\wedge\dots \wedge \mathrm{d}x^{k_{p}}~\in\Lambda^{p}N
\end{aligned}
\end{equation}
$$
Všimněme si, že jsou indexy báze stejné, což nemění nic na tom, že jsou indexy omezeny $\hat{n}$.

## Integrování TAS forem na podvarietě
Prvně musíme provést restrikci těchto forem $\alpha \in\Lambda^{m}M$ na podvarietu, na které budeme integrovat. $\alpha|_{N}\in\Lambda^{n}N$. Integraci forem, víme, že docílíme přemapováním na [integrovatelnou hustotu](Integrovatelné%20hustoty#D%20Tenzorové%20hustoty) $\mathfrak{a}=\iota_{+}\alpha \in \mathbf{H}N$. Formálně přes [hustotní duál](Divergence%20Tenzorových%20Hustot#D%20Hustotní%20duál) $\mathfrak{a}=\alpha\bullet\mathrm{d}\Sigma_{N}$, kde $\mathrm{d}\Sigma_{N}$ je **tečný** "plošný" element, tj. element, který vybírá tečné složky na podvarietě $N$. Je to tedy ekvivalent $\tilde{\varepsilon}^{-1}_{N}$.
$$
\begin{equation}
\begin{aligned}
\mathrm{d}\Sigma_{N}\in \mathbf{H}_{\mathscr{x}}N \otimes \mathbf{T}_{\mathscr{x}}^{[n]}M
\end{aligned}
\end{equation}
$$
V přizpůsobených souřadnicích je jeho podstava zcela zřejmá. Chápáno jako výběr vektorů velké variety.
$$
\begin{equation}
\begin{aligned}
\mathrm{d}\Sigma_{N}=(\mathrm{d}^{n}x)~\partial_{x^{\hat{n}+1}}\wedge\dots \wedge \partial_{x^{m}}
\end{aligned}
\end{equation}
$$
Dosazením do formálního zápisu $\mathfrak{a}=\alpha\bullet\mathrm{d}\Sigma_{N}=\alpha_{\hat{n}+1\dots \hat{n}+n}~\mathrm{d}^{n}x$, vzal normálové indexy a přeformuloval do řeči tečných na $N$.
$$
\begin{equation}
\begin{aligned}
\int_{\Omega\subset N}\alpha|_{N}=\int_{\Omega \subset N} \mathfrak{a}\bullet\mathrm{d}\Sigma_{N}=\int_{x[\Omega]}\alpha_{\hat{n}+1\dots \hat{n}+n}~\mathrm{d}^{n}x
\end{aligned}
\end{equation}
$$

**Příklad:** $N$ dim 1. Tehdy $\tau$ souřadnice na $N$, pak $\mathrm{d}\Sigma_{N}=\frac{ \partial  }{ \partial \tau }\mathrm{d}\tau$

## Integrování TAS tenz. hustot na podvarietě
Abstraktněji můžeme uvažovat o integraci [TAS tenz. hustot](Divergence%20Tenzorových%20Hustot#D%20Hustotní%20duál). Začněme přímo od duálního objektu představující tenzor hustoty na větší varietě $M$, $\mathfrak{a}\in\Lambda^{\#\hat{n}}M\longrightarrow \mathfrak{a}|_{N}$, který restrikujeme na $N$.
Aby bylo možné restrikovanou hustotu integrovat na podvarietě, tak musí $\mathfrak{a}|_{N}\in \mathbf{H}_{}M$. Toho docílíme vhodnou definicí, formálním zavedením **normálového** plošného elementu $\tilde{\mathrm{d}S_{N}}$:
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}|_{N}\equiv\mathfrak{a}\bullet{} &\tilde{\mathrm{d}S_{N}}=:\alpha\bullet\mathrm{d}\Sigma_{N}\\
\mathfrak{a}&=\#\alpha
\end{aligned}
\end{equation}
$$
Z definice si všimněme, že musí odstranit hustotní část $M$ ($\mathfrak{a}$) a nahradit za normálovou hustotu v $N$, čili $\tilde{\mathrm{d}S_{N}}\in \mathbf{H}_{\mathscr{x}}N\otimes \mathbf{H}^{-1}_{\mathscr{x}}M\otimes \mathbf{N}_{\mathscr{x}}^{*}N$. Z rovnice duality výše, taky vidíme, že relace plošných hustot je zprostředkovaná skrze $\#$, neboli přes [relaci duality](Divergence%20Tenzorových%20Hustot#^q9zd3w)
$$
\begin{equation}
\begin{aligned}
\tilde{\mathrm{d}S_{N}}:=\tilde{\varepsilon}_{M}\bullet \mathrm{d}\Sigma_{N}
\end{aligned}
\end{equation}
$$
V přizpůsobených souřadnicích $\tilde{\mathrm{d}S_{N}}=\mathrm{d}^{n}x~(\mathrm{d}^{m}x)^{-1}~(\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{\hat{n}+n})~(\partial_{x^{\hat{n}+1}}\wedge\dots \wedge \partial_{x^{m}})=\mathrm{d}^{n}x~\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{\hat{n}}$.
$$
\begin{equation}
\begin{aligned}
\int_{\Omega\subset N}\mathfrak{a}|_{N}=\int_{\Omega\subset N}\mathfrak{a}\bullet\tilde{\mathrm{d}S_{N}}=\int_{x[\Omega]}\mathfrak{a}_{x}^{1\dots \hat{n}}~\mathrm{d}^{n}x
\end{aligned}
\end{equation}
$$

Všimněme si, že nyní u $\mathfrak{a}$ integrujeme **normálové** komponenty, zatímco dříve to byly u $\alpha$ **tečné** komponenty.

**Příklad:** Ve 3D $\mathrm{d}\vec{S}=\vec{n}\mathrm{d}S$.
Případně, když by kodimenze byla $\hat{n}=1$, tj. když $\dim N=n=m-1$, pak
$\tilde{\mathrm{d}S_{N}}=\frac{\mathrm{d}^{m-1}x}{\mathrm{d}^{m}x}\mathrm{d}x^{1}$.
A úplně speciálně $N=M$, tj. $\hat{n}=0$, pak musí $\tilde{\mathrm{d}S_{N}}=1$.

**Poznámka:**  
Duální vztah se propisuje jen významově do komponent. $\alpha \overset{\#}\longleftrightarrow\mathfrak{a}$: $\mathfrak{a}_{x}^{\hat{K}}={\tilde{\varepsilon}_{x}^{-1}}^{\hat{K}K}\alpha_{K}$, přičemž $\hat{K}$ a $K$ jsou vzájemně komplementární množiny indexů, ${K}$ je soubor normálových indexů hustoty $1,\dots,\hat{n}$ . Zatímco $\hat{K}$ je soubor tečný indexů formy $\hat{n}+1,\dots ,\hat{n}+n$. Komponenty objektů jsou stejné $\mathfrak{a}_{x}^{n_{1}\dots \hat{n}}=\alpha_{\hat{n}+1\dots \hat{n}+n}$ jen je jiný jejich význam. 

## Integrální věty
Nutné podotknout, že je lze zformulovat bez jakékoliv metriky. Jedná se pouze o hru hustot, forem a restrikcí na podvariety.
### Věta: Stokesova věta pro TAS formy
$$
\begin{equation}
\begin{aligned}
\int_{\Omega}\mathrm{d}\omega=\int_{\partial \Omega}\omega|_{\partial \Omega}\\
\omega \in\mathcal{A}^{m-1}M

\end{aligned}
\end{equation}
$$
**Dk:** Sketch důkazu.
Typicky rozdělíme oblast zájmu $\Omega$ na části, kde máme vhodnou souřadnicovou mapu $U$,  tj. části (---). Té pak najdeme přizpůsobené souřadnice $x^{m}$. Integrace $\mathrm{d}\omega$ je pak integrace komponent $\sim\omega_{[1\dots m-1,m]}$, blíže $\sum \omega_{\underbrace{ 1\dots m }_{ m-1},k}$ , kde $k$-index chybí a je derivace. Když budu komponenty integrovat vždy se objeví souřadnice v $k$-směru , která nebude zahubena přizpůsobenými tečnými souřadnicemi a je ponechána pouze ta normálová. Tehdy je to jednoduchá integrace ve smyslu Newtonova rozdílu komponenty na hranicích. A to je ono, přizpůsobené souřadnice zařídí restrikci $\omega|_{\partial \Omega}$.
![[Stokes.svg|500]]
### Lemma: Zobecnění Stokesovy věty na podvariety
$$
\begin{equation}
\begin{aligned}
\int_{\Omega\subset N}\mathrm{d}\alpha|_{N}=\int_{\partial \Omega}\alpha|_{\partial \Omega}\\
\alpha \in \mathcal{A}^{m-1}M
\end{aligned}
\end{equation}
$$

![[StokesPodvariety.svg|500]]
Další případy už uvažujeme v tomto kontextu postupných restrikcí.
### Věta: Stokesova věta s rotací
$$
\begin{equation}
\begin{aligned}
\int_{V\subset N}\tilde{\text{rot}}\alpha\bullet\tilde{\mathrm{d}S_{N}}=\int_{\partial V}\alpha\bullet\mathrm{d}\Sigma_{\partial V}
\end{aligned}
\end{equation}
$$

**Dk:**
Přeformulováním vidíme [Stokesovu větu](Integrální%20věty#Věta%20Stokesova%20věta%20pro%20TAS%20formy):
$$
\begin{equation}
\begin{aligned}
\text{LS}=\int_{V\subset N}\underbrace{ \tilde{\text{rot}}\alpha }_{ \#\mathrm{d}\alpha }\bullet\tilde{\mathrm{d}S_{N}}=\int_{V\subset N}\mathrm{d}\alpha\bullet{\mathrm{d}\Sigma_{N}}=\int_{V\subset N}\mathrm{d}\alpha|_{N}
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\text{PS}=\int_{\partial V}\alpha\bullet{\mathrm{d}\Sigma_{\partial V}}=\int_{\partial V}\alpha|_{\partial V}
\end{aligned}
\end{equation}
$$
### Věta: Gaussova věta
$$
\begin{equation}
\begin{aligned}
\int_{V\subset N} (\text{div}\mathfrak{a})\bullet\tilde{\mathrm{d}S_{N}}=\int_{\partial V}\mathfrak{a}\bullet\tilde{\mathrm{d}S_{V}}
\end{aligned}
\end{equation}
$$

**Dk:**
opět se jedná o [Stokesovu větu](Integrální%20věty#Věta%20Stokesova%20věta%20pro%20TAS%20formy): 
$$
\begin{equation}
\begin{aligned}
\text{LS}=\int_{V\subset N} (\text{div}\mathfrak{a})\bullet\tilde{\mathrm{d}S_{N}}=\int_{V\subset N}\#\mathrm{d}\#\mathfrak{a}\bullet\tilde{\mathrm{d}S_{N}}=\int_{V\subset N}\#\mathrm{d}\alpha\bullet\tilde{\mathrm{d}S_{N}}=\int_{V\subset N}\mathrm{d}\alpha\bullet\tilde{\mathrm{d}\Sigma_{N}}\\
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\text{PS}=\int_{\partial V}\mathfrak{a}\bullet\tilde{\mathrm{d}S_{V}}=\int_{\partial V}\alpha \bullet\tilde{\mathrm{d}\Sigma_{\partial V}}=\int_{\partial V}\alpha|_{\partial V}
\end{aligned}
\end{equation}
$$

**Speciálně:** $n=m,\,N=M$, možná více známe
$$
\begin{equation}
\begin{aligned}
\int_{V}\mathrm{div}\mathfrak{a}=\int_{\partial V}\mathfrak{a}\bullet\tilde{\mathrm{d}S_{\partial V}}
\end{aligned}
\end{equation}
$$
## S metrickou strukturou na rozloučenou

Máme na varietě [metrickou strukturu](Metrická%20Struktura#Metrická%20struktura), a tedy $g$ metriku na $M$. 
Nyní můžeme jednoznačně určit normálové vektory podvariety $N$ jako $n:\,\, n\cdot g\cdot t=0,\,\forall t$ tečné. 
Analogicky tečné kovektory k $N$ jako $\sigma:\,\,\sigma \cdot g^{-1}\cdot \nu=0,\,\forall \nu$ tečné kovektory
nebo přes podmínku $\sigma \cdot n=0,\,\forall n$ normálové vektory.

### Nadplochy $\dim N=n=m-1$, kodimenze $\hat{n}=1$
Rozštěpení metriky, pomocí normálové $1$-formy $\nu$ a normálový vektor $n$. Pokud je podvarieta $N=\mathrm{\partial}\Omega$, hranice oblasti, pak $\nu$ nazveme „vnější normálou” a $n$ míří ven, $n\cdot \nu=1$ ($n=\pm g^{-1}\nu$).

**Rovnice na rozmyšlení:**
- $g=\pm \nu\nu+q$  $\text{sign}[g]=s_{M}\equiv \pm s$. Bude poučné vidět, kde mohou znaménka vyskakovat.
- $g^{-1}=\pm nn+q^{-1}$.
Levi-Civitův tenzor
- $\varepsilon_{M}=\nu \wedge\varepsilon_{N}$, a platí při zvedání indexu $\varepsilon_{M}\bullet\varepsilon^{\sharp}_{M}=s_{M}\,\,\varepsilon_{N}\bullet\varepsilon^{\sharp}_{N}=s_{N}$, přibude znaménko.
Integrovatelné hustoty
- $\text{g}^{1/2}=|\text{Det}g|^{\frac{1}{2}}=|\varepsilon_{M}|\,\,\,\mathscr{q}^{1/2}=|\text{Det}q|^{\frac{1}{2}}=|\varepsilon_{N}|=|\det q_{ab}|~\mathrm{d}^{m-1}x$.
Tenzor orientace
- $\tilde{\varepsilon}_{M}=\varepsilon_{M}~\text{g}^{-1/2}=\nu \wedge\varepsilon_{N}~\text{g}^{-1/2}\,\,\,\tilde{\varepsilon}^{-1}_{M}=s_{M}\varepsilon_{M}^{\sharp}~\text{g}^{1/2}=s_{N}n\wedge\varepsilon_{N}^{\sharp}~\text{g}^{1/2}$
- $\tilde{\varepsilon}_{N}=\varepsilon_{N}~\mathscr{q}^{-1/2}\,\,\,\tilde{\varepsilon}_{N}^{-1}=s_{N}\varepsilon_{N}^{\sharp}\mathscr{q}^{1/2}$
Elementy
- $\mathrm{d}\Sigma_{N}=s_{N}\varepsilon^{\sharp}\mathscr{q}^{1/2}$
	- $\alpha\bullet\tilde{\mathrm{d}\Sigma_{N}}=s_{N}\varepsilon_{N}\bullet\alpha \mathscr{q}^{1/2}=a\mathscr{q}^{{1}/{2}}$
	- čili $\alpha|_{N}=a\varepsilon_{N}$
- $\tilde{\mathrm{d}S_{N}}=\tilde{\varepsilon}_{M}\bullet{\mathrm{d}\Sigma_{N}}=s_{N}\mathscr{q}^{1/2}~\text{g}^{-1/2}\varepsilon_{M}\bullet\varepsilon_{N}^{\sharp}$
- $\tilde{\mathrm{d}S_{N}}=\nu \mathscr{q^{1/2}}~\text{g}^{-1/2}$
- $\tilde{\mathrm{d}S_{N}}=~\text{g}^{1/2}\tilde{\mathrm{d}S_{N}}=\nu \mathscr{q}^{1/2}$
- $a~\text{g}^{1/2}\bullet\tilde{\mathrm{d}S_{N}}=\mathfrak{a}\bullet\tilde{\mathrm{d}S_{N}}=a\cdot \nu~\mathscr{q}^{1/2}$
Gaussova věta
- $\int_{\Omega}(\nabla \cdot a)~\text{g}^{1/2}=\int_{\partial \Omega}a~{\mathrm{d}S_{N}}=\int_{\partial \Omega}a\cdot \nu ~\mathscr{q}^{1/2}$

