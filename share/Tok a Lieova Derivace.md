---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 12
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

# Tok a Lieova derivace
### D: Tok
Tok na $M$ je 1-par grupa difeomorfizmů na $M$.
$$
\begin{equation}
\begin{aligned}
\phi_{\alpha}\in\text{Diff}M,\,\,\,\alpha \in\mathbb{R}
\end{aligned}
\end{equation}
$$
s grupovou strukturou:
$$
\begin{equation}
\begin{aligned}
\phi_{\alpha}\circ\phi_{\beta}=\phi_{\alpha+\beta},\,\,\phi_{0}=\text{id},\,\,\phi_{-\alpha}=\phi^{-1}_{\alpha}
\end{aligned}
\end{equation}
$$
_Interpretace:_ Skrze bod $\mathscr{x}$ prochází orbita, na které se pohybujeme $\phi_{\alpha}$ pomocí parametru $\alpha$. Pro každý bod z nějakého okolí. Tak stejně pro jejich push-forward.

### D: Generátor toku
Jakožto malá orbita, posutím o malé $\alpha$. Jako vektor
$$
\begin{equation}
\begin{aligned}
a|_{\mathscr{x}}=\frac{\mathrm{D}}{\mathrm{d}\alpha}\phi_{\alpha}\mathscr{x}\bigg|_{\mathscr{\alpha=0}} 
\end{aligned}
\end{equation}
$$

Vztah mezi generátorem a tokem je jednoznačný.

### Lemma:
$\phi_{\alpha*}a=a$
__Dk:__
Vyčíslíme v bodě: $(\phi_{\alpha*}a)(\phi_{\alpha}\mathscr{x})=\phi_{\alpha*}(a(\mathscr{x}))\overset{\text{def}}=\phi_{\alpha*}\frac{\mathrm{D}}{\mathrm{d}\varepsilon}\phi_{\varepsilon}\mathscr{x}\bigg|_{\varepsilon=0}=\frac{\mathrm{D}}{\mathrm{d}\varepsilon}\phi_{\alpha*}\phi_{\varepsilon}\mathscr{x}\bigg|_{\varepsilon=0}=:a(\phi_{\alpha}\mathscr{x})$

Derivování tenzorových polí. Musíme dbát, že tenzor nelze odečítat v různých bodech. Zadáním celého [toku](#D%20Tok) skrz [generátor](#D%20Generátor%20toku), podle kterých budeme přenášet tenzorovou strukturu:
![[Derivace tenzorů.svg|400]]

### D: Lieova derivace
Derivace podle vektorového pole z tenzorového pole:
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}A|_{\mathscr{x}}:=\frac{\mathrm{d}}{\mathrm{d}\varepsilon}\phi_{-\varepsilon*}\left(A\left(\phi_{\varepsilon}\mathscr{x}\right)\right)\big|_{\varepsilon=0}
\end{aligned}
\end{equation}
$$
Obecně na tenzorové pole:
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}A:=-\frac{\mathrm{d}}{\mathrm{d}\varepsilon}\phi_{\varepsilon*}A\big|_{\varepsilon=0}
\end{aligned}
\end{equation}
$$
### Věta: Vlastnosti Lieovy derivace
- $\phi_{\alpha*}\circ\phi_{\beta*}=\phi_{\alpha+\beta*}$
- $\phi_{0*}=\text{id}$
- $\frac{\mathrm{d}}{\mathrm{d}\varepsilon}\phi_{\varepsilon*}\bigg|_{\varepsilon=0}=-\mathcal{L}_{a}$ Lieova derivace je generátor (akce) push-forwardu.
- $\frac{\mathrm{d}}{\mathrm{d}\alpha}\phi_{\alpha*}=-\phi_{\alpha*}\mathcal{L}_{a}$
Tyto vlastnosti vedou jednoznačně na Exponenciální řešení
$$
\begin{equation}
\begin{aligned}
\phi_{\alpha*}=\exp[-\alpha\mathcal{L}_{a}]
\end{aligned}
\end{equation}
$$
- $\mathcal{L}_{u}(A+rB)=\mathcal{L}_{u}A+r\mathcal{L}_{u}B;\,\,\,r\in \mathbb{R}$
- $\mathcal{L}_{u}(A\otimes B)=(\mathcal{L}_{u}A)\otimes B+A\otimes \mathcal{L}_{u}B$
- $\mathcal{L}_{u}(\mathrm{C}A)=\mathrm{C}\mathcal{L}_{u}A$
- $\mathcal{L}_{u}f=u[f];\,\,\,f\in \mathcal{F}M$
Takže je to derivace.
- $\mathcal{L}_{u}\mathrm{~d}f=\mathrm{~d}\mathcal{L}_{u}f$
důsledky:
- $\mathcal{L}_{u}(a\cdot \omega)=(\mathcal{L}_{u}a)\cdot \omega+a\cdot \mathcal{L}_{u}\omega$
- $\mathcal{L}_{u}v=[u;v]$ Lieova závorka
__Dk:__
Lieova závorka, akorát [Cartanův vzorec](Vnější%20kalkulus#Cartanovy%20vzorce) (s $\gamma=\mathrm{~d}f$)
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{u}(v\cdot\mathrm{d}f)
&=u\cdot \mathrm{d}(v\cdot \mathrm{d}f)
=(\mathcal{L}_{u}v)\cdot \mathrm{d}f+v\cdot \mathcal{L}_{u}\mathrm{~d}f\\
&=(\mathcal{L}_{u}v)\cdot \mathrm{d}f+v\cdot \mathrm{d}(u\cdot \mathrm{d}f)\\
&=u\cdot \mathrm{d}(v\cdot \mathrm{d}f)+v\cdot \mathrm{d}(u\cdot \mathrm{d}f)\\
&=[u;v]\cdot\mathrm{d}f
\end{aligned}
\end{equation}
$$

### Věta: $\mathcal{L}_{a}$ určená vlastnostmi
- tenzorová derivace $\implies$ určená akcí na $\mathcal{F}M$ a $\mathcal{T}_{}M$
- na $\mathcal{F}M:$ jako derivace podle $a$
- na $\mathcal{T}_{}M:$ jako $[a;\cdot]$

### Věta: Linearita, ale ne $\mathcal{F}M$-linearita $\mathcal{L}_{}$
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{u+rv}=\mathcal{L}_{u}+r\mathcal{L}_{v},\,\,\,r\in \mathbb{R},
\end{aligned}
\end{equation}
$$
ale na fcích
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{fu}=d\mathcal{L}_{u}+\mathbf{L},
\end{aligned}
\end{equation}
$$
kde $\mathbf{L}$ je pseudoderivace, není ultralokální (přirozeně, jelikož potřebujeme znát informaci i z okolí). Navíc
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{[u;v]}=\mathcal{L}_{u}\mathcal{L}_{v}-\mathcal{L}_{v}\mathcal{L}_{u}
\end{aligned}
\end{equation}
$$
__Dk:__
1) 
Definujme speciální tenzorovou derivaci
$$
\begin{equation}
\begin{aligned}
\mathbf{L}:=\mathcal{L}_{u+rv}-\mathcal{L}_{u}-r\mathcal{L}_{v},
\end{aligned}
\end{equation}
$$
kterou aplikujeme na funkce
$$
\begin{equation}
\begin{aligned}
\mathbf{L}f=(u+rv)[f]-u[f]-rv[f]=0
\end{aligned}
\end{equation}
$$
a jedná se o pseudoderivaci, kterážto je určena akcí na vektorech
$$
\begin{equation}
\begin{aligned}
\mathbf{L}w=[u+rv;w]-[u;w]-r[v;w]=0.
\end{aligned}
\end{equation}
$$
Tedy je navíc identicky nula. $\mathbf{L}=0$

2) 
Definujme speciální tenzorovou derivaci
$$
\begin{equation}
\begin{aligned}
\mathbf{L}:=\mathcal{L}_{fu}-f\mathcal{L}_{u},
\end{aligned}
\end{equation}
$$
kterou aplikujeme na $\mathcal{F}M$
$$
\begin{equation}
\begin{aligned}
\mathbf{L}h=fu[h]-fu[h]=0,
\end{aligned}
\end{equation}
$$
takže je pseudoderivace a opět hledáme akci na vektorech
$$
\begin{equation}
\begin{aligned}
\mathbf{L}w=\mathcal{L}_{fu}w-f\mathcal{L}_{u}w&=[fu;w]-f[u;w]\\
&=-u~ w[f]+f[u;w]-f[u;w]\\
&=-(u\mathrm{~d}f)\cdot w\\
\end{aligned}
\end{equation}
$$
Z toho vidíme akci pseudoderivace na formách:
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{fu}\alpha&=f\mathcal{L}_{u}\alpha+\mathbf{L}\alpha\\
&=f\mathcal{L}_{u}\alpha-\alpha \cdot(-u\mathrm{~d}f)
\end{aligned}
\end{equation}
$$

### Věta: Lie der. působení na Lie závorku
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{w}[u;v]=[\mathcal{L}_{w}u,v]+[u;\mathcal{L}_{w}v]
\end{aligned}
\end{equation}
$$
__Dk:__ Jacobiho identita

### Věta: Lie derivace na komponenty tenzoru
Mějme souřadnice $x^{a}$.
$$
\begin{equation}
\begin{aligned}
\left( \mathcal{L}_{\frac{ \partial  }{ \partial x^{i} } }A \right)^{a\dots}_{b\dots}=A^{a\dots}_{b\dots,i}
\end{aligned}
\end{equation}
$$
__Dk:__
Protože $A=A^{a\dots}_{b\dots}~\frac{ \partial  }{ \partial x^{a} }\cdots \mathrm{~d}x^{b}\cdots$, pak
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{\frac{ \partial  }{ \partial x^{i} } }A
&=\mathcal{L}_{\frac{ \partial  }{ \partial x^{i} } }\left( A^{a\dots}_{b\dots}~\underbrace{ \frac{ \partial  }{ \partial x^{a} }\cdots }_{ [\cdot;\cdot]=0 } \underbrace{ \mathrm{~d}x^{b}\cdots }_{ 0 } \right)\\
&=A^{a\dots}_{b\dots,i}~\frac{ \partial  }{ \partial x^{a} }\cdots \mathrm{~d}x^{b}\cdots
\end{aligned}
\end{equation}
$$


## Vztah Lie derivace a AS forem
### Věta: Jednoznačnost Lie derivace na AS forem
Následující vlastnosti určují Lie derivaci na AS formách jednoznačně.
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}:\mathcal{A}^{p}M\to\mathcal{A}^{p}M
\end{aligned}
\end{equation}
$$
$\mathcal{L}_{a}(\omega+\sigma)=\mathcal{L}_{a}\omega+\mathcal{L}_{a}\sigma$,
$\mathcal{L}_{a}(\omega \wedge\sigma)=(\mathcal{L}_{a}\omega)\wedge\sigma+\omega\wedge\mathcal{L}_{a}\sigma$,
a již známé
$\mathcal{L}_{a}f=a[f]$,
$\mathcal{L}_{a}\mathrm{~d}f=\mathrm{~d}\mathcal{L}_{a}f$.

### Věta: Cartanova identita
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}\omega=a\cdot \mathrm{d}\omega+\mathrm{d}(a\cdot \omega)
\end{aligned}
\end{equation}
$$
neboli $\mathcal{L}_{a}=i_{a}\mathrm{~d}+\mathrm{d}~i_{a}$

__Dk:__
Pokud splňuje vlastnosti, které určují Lieovu derivaci jednoznačně.
Netriviální Leibniz:
$$
\begin{equation}
\begin{aligned}
&\,\,\,a\cdot \mathrm{~d}(\omega \wedge\sigma)+\mathrm{~d}(a\cdot(\omega \wedge\sigma))=\\
&= a\cdot (\mathrm{~d}\omega \wedge \sigma+(-1)^{p}\omega \wedge \mathrm{d}\sigma)+\mathrm{~d}((a\cdot \omega)\wedge\sigma+(-1)^{p}\omega \wedge(a\cdot \sigma))\\
&=(a\cdot \mathrm{~d}\omega+\mathrm{~d}(a\cdot \omega))\wedge\sigma+(-1)^{2p}\omega \wedge(a\cdot \mathrm{~d}\sigma+\mathrm{~d}(a\cdot\sigma))\\
&\,\,\,+\cancel{ (-1)^{p+1}\mathrm{~d}\omega \wedge(a\cdot\sigma) }+\cancel{ \cancel{ (-1)^{p}(a\cdot \omega)\wedge \mathrm{~d}\sigma } }\\
&\,\,\,+\cancel{ (-1)^{p}\mathrm{~d}\omega \wedge(a\cdot\sigma) }+\cancel{ \cancel{ (-1)^{p+1}(a\cdot \omega)\wedge \mathrm{~d}\sigma} }
\end{aligned}
\end{equation}
$$

### Věta: Lie komutuje s vnější derivací
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}\mathrm{~d}=\mathrm{d}\mathcal{L}_{a}
\end{aligned}
\end{equation}
$$
__Dk:__ S [Cartanovou identitou](#Věta%20Cartanova%20identita):
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}\mathrm{~d}\omega
&=a\cdot \mathrm{~d}\mathrm{~d}\omega+\mathrm{~d}(a\cdot \mathrm{~d}\omega)\\
&=\mathrm{d}(a\cdot\mathrm{~d}\omega+\mathrm{~d}(a\cdot \omega))\\
&=\mathrm{d}~\mathcal{L}_{a}\omega
\end{aligned}
\end{equation}
$$

### Věta: Lie s insertem
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}i_{b}-i_{b}\mathcal{L}_{a}=i_{[a;b]}
\end{aligned}
\end{equation}
$$
__Dk:__
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}i_{b}\omega=\mathcal{L}_{a}(b\cdot \omega)=(\mathcal{L}_{a}b)\cdot \omega+b\cdot \mathcal{L}_{a}\omega=[a;b]\cdot \omega= i_{b}\mathcal{L}_{a}\omega
\end{aligned}
\end{equation}
$$
### Věta: neUltralokalita Lie pro formy
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{fa}\omega=f\mathcal{L}_{a}\omega+\mathrm{d}f\wedge(a\cdot \omega)
\end{aligned}
\end{equation}
$$
__Dk:__ Opět [Cartanova identita](#Věta%20Cartanova%20identita).
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{fa}\omega
&=fa\cdot \mathrm{d}\omega+\mathrm{~d}(fa\cdot \omega)
=f(a\cdot \mathrm{d}\omega+\mathrm{~d}(a\cdot \omega))+\mathrm{~d}f\wedge(a\cdot \omega)\\
&= f\mathcal{L}_{a}\omega+\mathrm{d}f\wedge(a\cdot \omega)
\end{aligned}
\end{equation}
$$










