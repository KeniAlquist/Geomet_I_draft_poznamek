---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 11
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

# Podvarieta a tečné distribuce
## Podvarieta
### D: Podvarieta dimenze $n\leq m$
Zobrazení 
$\iota:\tilde{N}\to M$ je lokálně prosté když
- $\iota_{*}$ push-forward je prostý (nedegeneruje směry)
![[Podvarieta.svg|400]]
($\iota \tilde{N}=N$)

### D: Vnoření a vložení
_Vnoření_ je $\iota$ lokálně prosté zobrazení.
_Vložení_ je $\iota$ prosté zobrazení.
![[vnoření.svg|400]]
(není vložení, jen vnoření)

### D: Přizpůsobené souřadnice
$\tilde{N}$   $y^{i}$    $i=1,\dots ,n$   ;   $M$   $x^{a}$   $a=1,\dots,m$
pak přizpůsobenými souřadnicemi myslíme souřadnice $M$, které
$$
\begin{equation}
\begin{aligned}
y^{i}&=x^{i},\,\,\,i=1,\dots ,n\\
\text{konst.}&=x^{p},\,\,\,p=n+1,\dots,m
\end{aligned}
\end{equation}
$$
### D: Tečné struktury
…k podvarietě a normálové kovektory.

##### Tečné vektory k $N$  
(tj. ta už v M)
$$
\begin{equation}
\begin{aligned}
\iota_{*}: \mathbf{T}_{\mathscr{\tilde{x}}}\tilde{N}\to \mathbf{T}_{\mathscr{x}}N\subset \mathbf{T}_{\mathscr{x}}M
\end{aligned}
\end{equation}
$$

##### Restrikce formy
(!!$\mathbf{T}_{\mathscr{x}}N\subset \mathbf{T}_{\mathscr{x}}M$. To samé __nelze__ říct o kovektorech, neboť ty jsou definované akcí na obecně větším počtu vektorů než je na $N$!!) Na formy jdeme obecně z druhé strany a děláme jejich restrikci.

$$
\begin{equation}
\begin{aligned}
\iota^{*}:\mathbf{T}_{\mathscr{x}}^{*}M\to \mathbf{T}^{*}_{\mathscr{\tilde{x}}}\tilde{N}
\end{aligned}
\end{equation}
$$
ozn. $\omega|_{{N}}=\iota^{*}\omega$

##### Normálové kovektory
$\omega \in \mathbf{N}_{\mathscr{x}}^{*}N\subset \mathbf{T}_{\mathscr{x}}^{*}M\iff\omega|_{N}=0$.

Intuitivně rozumíme, že objekty z $\mathbf{N}^{*}_{\mathscr{x}}N$ jsou v jakém si smyslu „paralelní” s $\mathbf{T}_{\mathscr{x}}M$, neboli že kontrakce dá $0$.
Dualita $\mathbf{T}_{\mathscr{x}}M$ a $\mathbf{N}^{*}_{\mathscr{x}}N$:
$$
\begin{equation}
\begin{aligned}
\omega \in \mathbf{N}^{*}_{\mathscr{x}}N &\iff \forall a\in \mathbf{T}_{\mathscr{x}}N:\omega \bullet a=0 \\
\text{nebo opačně}\\
a \in \mathbf{T}_{\mathscr{x}}N &\iff \forall \omega\in \mathbf{N}^{*}_{\mathscr{x}}N:\omega \bullet a=0
\end{aligned}
\end{equation}
$$

##### Tečné struktury v souřadnicích:
Pro prvek z originální $\mathbf{T}\tilde{N}\ni \tilde{a}$, který má souřadnice: $\tilde{a}=a^{i}\frac{ \partial  }{ \partial y^{i} }$. Můžeme push-forwardem
$a=\iota_{*}\tilde{a}\in \mathbf{T}N$, přiřadit přizpůsobené souřadnice $a=\sum_{i}^{n}a^{i}\partial_{x^{i}}$ .

Pro prvek z cílové $\mathbf{T}^{*}_{}M\ni\omega$, která je v souřadnicích: $\omega=\omega_{a}\pu{~d}x^{a}$. Můžeme restrikcí formy, tj. pull-backem
$\iota^{*}\omega=\omega|_{N}=\tilde{\omega}\in \mathbf{T}^{*}_{}\tilde{N}$, přiřadit $\tilde{\omega}=\sum_{i}^{p}\omega_{i}\pu{~d}y^{i}$. Normální kovektory budou právě ty, které chybí:
$$
\begin{equation}
\begin{aligned}
\omega \in \mathbf{N}^{*}N\,\,\omega=\sum_{p=n+1}^{m}\omega_{p}\pu{~d}x^{p}
\end{aligned}
\end{equation}
$$
## Distribuce
### D: $\Delta$ je Distribuce (…podprostorů tečných vektorů dim $n$)
Soubor podprostorů $\Delta_{\mathscr{x}}\subset \mathbf{T}_{\mathscr{x}}M:\forall \mathscr{x}\in M,\pu{ dim }\Delta_{\mathscr{x}}=n$.
Je generovaná hladkými vektorovými poli $a_{j}\in \mathcal{T}_{}M,j=1,\dots,n$. (není nutné celé $\mathcal{T}_{}M$ ale i na okolí každého bodu.)
Říkáme, že $a\in \mathcal{T}_{}M$ patří do $\Delta$, když $\forall \mathscr{x}:a|_{\mathscr{x}}\in\Delta_{\mathscr{x}}$.

### D: $\Delta^{*}$ je Kodistribuce (…podprostorů normálových kovektorů dim $m-n$)
Soubor podprostorů $\Delta^{*}_{\mathscr{x}}\subset \mathbf{T}_{\mathscr{x}}^{*}M: \forall \mathscr{x}\in M,\pu{ dim }\Delta^{*}_{\mathscr{x}}=m-n$.
Je generovaná hladkými poli $\alpha^{p}\in \mathcal{T}^{*}_{}M,p=n+1,\dots,m$ (opět obecně ne nutně celý prostor kovektorových polí.)
Říkáme, že $\alpha \in \mathcal{T}^{*}M$ patří do $\Delta^{*}$ když $\forall \mathscr{x}:\alpha|_{\mathscr{x}}\in\Delta_{\mathscr{x}}^{*}$.

### D: $\Delta$ a $\Delta^{*}$ jsou komplementární
pokud $\pu{ dim }\Delta=n,\,\pu{ dim }\Delta^{*}=m-n$.
Pak pro 
$\forall a\in\Delta_{\mathscr{x}},\forall\alpha \in\Delta^{*}_{\mathscr{x}}:a\cdot\alpha=0$, platí:
$a\in\Delta \iff \forall\alpha \in\Delta^{*}:\alpha \cdot a=0$ a
$\alpha\in\Delta^{*} \iff \forall a \in\Delta:\alpha \cdot a=0$.
Odteď už uvažujeme jen komplementární.
### D: Nulovost restrikce formy $\omega \in \mathcal{T}_{k}^{0}M$
Restrikce formy na $\Delta$ znamená a je nulová když:
$$
\begin{equation}
\begin{aligned}
\forall a^{1},\dots,a^{k}\in\Delta:\omega(a^{1},\dots,a^{k})=0 \iff \omega|_{\Delta}=0
\end{aligned}
\end{equation}
$$
pozn.: jde to s formou jde to s kovektorem.

### D: Involutivita $\Delta$
Pakliže $[\Delta;\Delta]=\Delta$
přesněji $\forall a,b\in \Delta:[a;b]\in\Delta$, 
ještě přesněji pomocí generátorů $[a_{i};a_{j}]=\sum_{j}^{n}f_{ii'}^{j}a_{j}$.

### D: $\Delta^{*}$ je diferenciální
pokud $\forall\alpha \in\Delta^{*}:(\pu{ d}\alpha)|_{\Delta}=0$.

Přesněji lze $\pu{ d}\alpha^{p}$ zapsat v $\pu{ d}\alpha^{p}=\sum_{q=n+1}^{m}\theta_{q}^{p}\wedge\alpha^{q}$, kde mi $p, q$ pouze číslují konkrétní formy z $\mathcal{T}^{*}_{}M$.
$\theta$ jsou obecné jakékoliv 1-formy, lze tedy $\pu{ d}\alpha^{p}$ vyjádřit pomocí $\theta$ jako kombinací souboru forem vně $\Delta^{*}$ ($\alpha^{q}$).

Komentář: Tedy Takto určené formy $\alpha^{p}$, které patří do $\Delta^{*}$ , mají vnější derivaci, která lze zapsat jako lineární kombinace $\alpha^{q}$ ležící mimo $\Delta^{*}$, proto derivace mizí na vektorových polích z $\Delta$: $\theta_{q}^{p}\wedge \alpha^{q}$ 2-forma – a tedy $\Delta^{*}$ je diferenciální systém.

### Věta: $\Delta$ je involutivní$\iff\Delta^{*}$ je diferenciální
Dk:
$\Delta^{*}$ je diferenciální tedy $(\pu{ d}\alpha^{p})|_{\Delta}=0$, jinak řečeno to je když pro $a_{i},a_{j}\in\Delta:0\overset!=a_{i}\cdot \pu{ d}\alpha^{p}\cdot a_{j}$

__Dk:__ Použitím Cartanova vzorce:
$$
\begin{equation}
\begin{aligned}
0&\overset!=a_{i}\cdot \pu{ d}\alpha^{p}\cdot a_{j}\\
&=a_{i}\cdot \pu{ d}(\alpha^{p}\cdot a_{j})-\pu{ d}(a_{i}\cdot \alpha^{p})\cdot a_{j}-[a_{i};a_{j}]\cdot \alpha^{p}\\
&=0-0-[a_{i};a_{j}]\cdot \alpha^{p}
\end{aligned}
\end{equation}
$$
ale $\alpha^{p}\in\Delta^{*}$, tedy $[a_{i};a_{j}]\in \Delta$, takže je $\Delta$ involutivní.

### D: $N$ integrabilní podvarieta
$N$ je __integrabilní podvarieta__ pokud:
$\forall \mathscr{x}\in N;a\in\Delta_{\mathscr{x}}\iff a\in\mathbf{T}_{\mathscr{x}}N$
ekvivalentně definováno pomocí kodistribucí
$\alpha \in\Delta_{\mathscr{x}}^{*}\iff\alpha \in \mathbf{N}_{\mathscr{x}}^{*}N$, tj. $\Delta^{*}_{\mathscr{x}}=\mathbf{N}_{\mathscr{x}}N,\forall \mathscr{x}\in N$.

### D: $\Delta^{*},\Delta$ integrabilní
[Distribuce](#D%20$%20Delta$%20je%20Distribuce%20(%E2%80%A6podprostor%C5%AF%20te%C4%8Dn%C3%BDch%20vektor%C5%AF%20dim%20$n$)) jsou __integrabilní__ pokud každým $\mathscr{x}\in M$ prochází nějaká [integrabilní podvarieta](#D%20$N$%20integrabiln%C3%AD%20podvarieta).
_komentář:_ pro soubor (ko)vektorů v každém $\mathscr{x},$ tj. distribuci, existuje [integrabilní podvarieta](#D%20$N$%20integrabiln%C3%AD%20podvarieta). [Distribuce](#D%20$%20Delta$%20je%20Distribuce%20(%E2%80%A6podprostor%C5%AF%20te%C4%8Dn%C3%BDch%20vektor%C5%AF%20dim%20$n$)) jsou si stále komplementární

### Věta: $\Delta,\Delta^{*}$ jsou integrabilní
$\iff$ $\exists$ souřadnice $x^{k}:$
- $\frac{ \partial  }{ \partial x^{i} },\,i=1,\dots,n$ generují $\Delta_{\mathscr{x}}$
- $\pu{ d}x^{p},\,p=n+1,\dots,m$ generující $\Delta_{\mathscr{x}}^{*}$
- $x^{p}=\text{konst.}~,$ pro $p=n+1,\dots,m$. Definují [integrabilní podvariety](#D%20$N$%20integrabiln%C3%AD%20podvarieta) (vrstvení).
- $x^{i}=\text{konst.}~,$ pro $i=1,\dots,n$. jsou souřadnice na [integrabilních podvarietách](#D%20$N$%20integrabiln%C3%AD%20podvarieta).
komentář: Jsou to tedy [přizpůsobené souřadnice](#D%20P%C5%99izp%C5%AFsoben%C3%A9%20sou%C5%99adnice) celé sadě podvariet určené $x^{p}=\text{konst.}$

## Věta: Frobeniova věta
$\Delta,\Delta^{*}$ je [integrabilní](#D%20$%20Delta%20%7B*%7D,%20Delta$%20integrabiln%C3%AD)
$\iff$
$\Delta$ je [involutivní](#D%20Involutivita%20$%20Delta$)
$\iff$
$\Delta^{*}$ je [diferenciální](#D%20$%20Delta%20%7B*%7D$%20je%20diferenci%C3%A1ln%C3%AD)

__Dk:__ 
$\Delta,\Delta^{*}$ je integrabilní $\implies \exists x^{p},~p=n+1,\dots m:\mathrm{d}x^{p}$ generují $\Delta^{*}$. 
Pak z toho plyne $\alpha^{p}$ generátory $\Delta^{*}$ musí být lineární kombinací $\mathrm{d}x^{p}:$
$$
\begin{equation}
\begin{aligned}
\alpha^{p}=\sum_{q=n+1}^{m} A^{p}_{q}\mathrm{d}x^{q}
\end{aligned}
\end{equation}
$$
pak
$$
\begin{equation}
\begin{aligned}
\mathrm{d}\alpha^{p}
&=\sum_{q=n+1}^{m}\mathrm{~d}A^{p}_{q}\wedge \mathrm{d}x^{q}+0\\
&=\sum_{r,q=n+1}^{m}\mathrm{~d}A^{p}_{q}\wedge {A^{-1}}^{q}_{r}~\alpha^{r}
=\sum_{r,q=n+1}^{m}\mathrm{~d}A^{p}_{q}~{A^{-1}}^{q}_{r}\wedge \alpha^{r}\\
&=:\sum_{r=n+1}^{m}\theta^{p}_{r}\wedge\alpha^{r}
\end{aligned}
\end{equation}
$$
$\impliedby$ skrze indukci a dlouze. Ale proč by ne...
$\Delta$ je involutivní, pro integrabilitu (z [podmínky integrability](#D%20$%20Delta%20%7B*%7D,%20Delta$%20integrabiln%C3%AD)) potřebujeme ukázat existenci souřadnic $x^{k}$ generující $\Delta^{*}$ jako $\mathrm{~d}x^{k}$. Indukčním přidáváním do distribuce:
$n=1:$ Nechť
$$
\begin{equation}
\begin{aligned}
a_{1} \text{ gen }\Delta \implies \exists x^{k}:a_{1}[x^{1}]=1,\,a_{1}[x^{p}]=0
\end{aligned}
\end{equation}
$$
tedy $\exists$ komplementární generátory $\mathrm{~d}x^{p}$ $\Delta^{*}$. (V 1D je každá distribuce integrabilní.)
$n-1 \to n:$ 
$$
\begin{equation}
\begin{aligned}
a_{j}\text{ gen }\Delta \text{ a zároveň z předpokladu } \Delta \text{ involutivní, tedy }[a_{i},a_{i'}]=f^{j}_{ii'} a_{j}
\end{aligned}
\end{equation}
$$
Definujeme novou sadu generátorů, odečtením projekce do směru 1. souřadnice ($a_{1}[x^{1}]=1$):
$$
\begin{equation}
\begin{aligned}
\bar{a}_{j}=a_{j}-a_{j}[x^{1}]a_{1};\,\,\bar{a}_{1}=a_{1}
\end{aligned}
\end{equation}
$$
Tahle nová kolekce 
- generuje $\Delta$
- zcela určitě generuje $\Delta$ involutivní.
- $\bar{a}_{j}[x^{1}]=0$ z konstrukce
- $\ [\bar{a}_{1};\bar{a}_{i}]=\cdots=\sum_{j=2}^{n}f^{j}_{1i}\bar{a}_{j}$
lze použít indukční předpoklad. $\exists x^{p}:\mathrm{~d}x^{p}$ generující $\Delta^{*}$ [komplementární](#D%20$%20Delta$%20a%20$%20Delta%20%7B*%7D$%20jsou%20komplement%C3%A1rn%C3%AD) k $\Delta$ generované $\bar{a}_{j}$.

## Příklady:
$m=3$, $n=2$. $M\,\,\,\rho,\varphi,z$
Mějme kodistribuci $\Delta^{*}:\alpha=\rho^{2}\mathrm{~d}\varphi+\mathrm{d}z$ (dim 2). je kodistribuce integrabilní?
Můžeme otestovat
1) [Involutivitu](#D%20Involutivita%20$%20Delta$) [komplementární](#D%20$%20Delta$%20a%20$%20Delta%20%7B*%7D$%20jsou%20komplement%C3%A1rn%C3%AD) $\Delta$.
2) [diferencialitu](#D%20$%20Delta%20%7B*%7D$%20je%20diferenci%C3%A1ln%C3%AD) $\Delta^{*}$.

Najděme prvky z [komplementární distribuce](#D%20$%20Delta$%20a%20$%20Delta%20%7B*%7D$%20jsou%20komplement%C3%A1rn%C3%AD), tzn.: $0\overset!=\alpha \cdot a$
$$
\begin{equation}
\begin{aligned}
0&\overset!= \rho^{2}a^{\varphi}+a^{z}\\
\implies&a_{1}= \frac{ \partial  }{ \partial \rho }\\
&a_{2}=-\rho^{2}\frac{ \partial  }{ \partial z }+\frac{ \partial  }{ \partial \varphi }  
\end{aligned}
\end{equation}
$$
ad 1)  $[\Delta;\Delta]=\Delta$:
$$
\begin{equation}
\begin{aligned}
\ [a_{1};a_{2}]=-2\frac{ \partial  }{ \partial z } \notin \text{span}\{ a_{1};a_{2} \} 
\end{aligned}
\end{equation}
$$
není involutivní.

ad 2) $\mathrm{~d}\alpha|_{\Delta} \overset?=0$:
$$
\begin{equation}
\begin{aligned}
\mathrm{~d}\alpha&=\mathrm{~d}(\rho^{2}\mathrm{~d}\varphi)+\cancelto{ 0 }{ \mathrm{d}(\mathrm{d}z) }\\
&=\mathrm{~d}(\rho^{2})\wedge \mathrm{d}\varphi +\rho^{2}\cancelto{ 0 }{ \mathrm{~d}(\mathrm{d}\varphi) }
=2\rho \mathrm{~d}\rho \wedge\mathrm{d}\varphi\\ 
\\
a_{1}\cdot \mathrm{d}\alpha \cdot a_{2}=2\rho&\neq{0}
\end{aligned}
\end{equation}
$$
takže $\Delta^{*}$ není diferenciální.

_LifeHack:_
Rychleji to lze poznat z toho, že podvarieta není integrabilní když nemá integrabilní křivky $(\rho(t),\varphi(t),z(t))\to\text{vektor }\dot{\gamma}$: $\alpha \cdot\dot{\gamma}=\rho^{2}\dot{\varphi}+\dot{z}=0$.
$\rho=\rho_{0},\,\varphi=\varphi \implies \dot{z}=-\rho^{2}\dot{\varphi}$. Křivky se neuzavírají po oběhu.

Speciální případy: $n=m-{1}$
$\Delta$ je generovaná $n-1$ vektorovými poli, pak jednoduše $\Delta^{*}$ jen jedním $\alpha$. A stačí ověřit, že $\mathrm{~d}\alpha=\theta \wedge\alpha$, pak je [diferenciální](#D%20$%20Delta%20%7B*%7D$%20je%20diferenci%C3%A1ln%C3%AD) a $\Delta$ je [involutivní](#D%20Involutivita%20$%20Delta$).

__Aplikace:__
[Frobeniova věta](#V%C4%9Bta%20Frobeniova%20v%C4%9Bta) na systém homogenních parciálních diferenciálních rovnic (HPDR):
$$
\begin{equation}
\begin{aligned}
\sum_{i=1}^{m}\alpha^{p}_{i}(x^{j}) \frac{ \partial X^{i} }{ \partial y^{k} }=0 
\end{aligned}
\end{equation}
$$
$X^{i}(y^{1},\dots,y^{n})$
Přeformulujeme:
$$
\begin{equation}
\begin{aligned}
M\,\text{dim}~m\,\,\,x^{j}\,\,\,j=1,\dots ,m\\
N\,\text{dim}~n\,\,\,y^{k}\,\,\,k=1,\dots ,n\\
\end{aligned}
\end{equation}
$$
Chceme, aby $X$ bylo vyjádření souřadnicově:
def [vnoření](#D%20Vno%C5%99en%C3%AD%20a%20vlo%C5%BEen%C3%AD) $\iota:N\hookrightarrow M$, takže technicky: $X=x\circ\iota\circ y^{-1}$.
Pak zavedli bychom $\Delta^{*}$ generovanou nějakými $\alpha^{p},\,p=n+1,\dots,m$. Pro ně by (díky komplementaritě) platilo: že kontrakce tečných vektorů z $N$ ($\partial_{y^{k}}$) vnořených do $M$ tj. $\iota_{*}\partial_{y^{k}}$, dává $0$:
$$
\begin{equation}
\begin{aligned}
0\overset{\text{def}}=\alpha^{p}\cdot\underbrace{ \left( \iota_{*}\frac{ \partial  }{ \partial y^{k} }  \right) }_{ \sum \frac{ \partial x^{i} }{ \partial y^{k} } \partial_{x^{i}} }=\sum_{i=1}^{m}\alpha^{p}_{i}(x^{j})\frac{ \partial x^{i} }{ \partial y^{k} } 
\end{aligned}
\end{equation}
$$
Což je přímo definice HPDR, a tedy $\Delta^{*}$ existuje. Řešením pak budou speciální $\alpha$: $\mathrm{d}\alpha^{p}|_{\Delta}=0$.



