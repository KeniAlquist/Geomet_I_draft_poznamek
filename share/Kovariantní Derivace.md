---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 14
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

# Kovariantní derivace
__Motivace:__
- chceme studovat změnu $\iff$ klid. Potřebujeme umět odečítat tenzory v různých bodech variety, tedy specifikovat přenos tenzoru podél nějaké křivky, např.: Paralelní přenos.
$\text{par}[z]:\mathbf{T}_{z(0)}M\to \mathbf{T}_{z(1)}M$, požadované vlastnosti:
- linearita,
- nezávislost na parametrizaci $z$,
- komutace s $\otimes$ a $\mathrm{C}$ + přenos čísla beze změny (to nám dovolí přenášet i formy),
- skládání $\mathrm{par}[z]=\mathrm{par}[z_{f}]\mathrm{par}[z_{i}]$,
- opačný směr $\mathrm{par}[z^{-1}]=\mathrm{par}^{-1}[z]$,
- přenos podél konst. křivky je identická transformace $\mathrm{par}[\text{konst.}]=id$. 
pak by měla jít reprezentovat tenzorově (ale stále nejednoznačně)
$(\mathrm{par}[z]a)^{\underline{ m }}=(\mathrm{par}[z])^{\underline{ m }}_{\underline{ \tilde{n} }}a^{\underline{ \tilde{n} }}$. Indexy v různých bodech.
<a id="kovariantni-derivace"></a>
### D: Kovariantní derivace (CD)
…Tenzorového pole $A$ ve směru $a$ v bodě $\mathscr{x}$.
$$
\begin{equation}
\begin{aligned}
\nabla_{a}A\in {\mathbf{T}_{\mathscr{x}}}^{k}_{l}M
\end{aligned}
\end{equation}
$$
splňující:
- $\nabla_{(a+rb)}A=\nabla_{a}A+r\nabla_{b}A;\,r\in \mathbb{R}$
- $\nabla_{a}(A+rB)=\nabla_{a} A+r\nabla_{a}B;\,r\in \mathbb{R}$
- $\nabla_{a}(AB)=A\nabla_{a}(B)+(\nabla_{a}A)B$ !!! Pořadí indexů $a$ je první !!!
- $\nabla_{a}\mathrm{C}A=\mathrm{C}\nabla_{a}A$
- na fce: $\nabla_{a}f:=a[f]\equiv a\cdot \mathrm{d}f$
(není určena jednoznačně. Zobecníme do každého bodu)

### D: Kovariantní diferenciál
$$
\begin{equation}
\begin{aligned}
\nabla_{a}A\in {\mathcal{T}}^{k}_{l}M;\,\,\,A\in \mathcal{T}_{l}^{k}M;\,\,\,a\in \mathcal{T}M\\
\\
\nabla_{a}A^{\underline{ k }\dots}_{\underline{ l }\dots}=a^{\underline{ m }}\nabla_{\underline{ m }}A^{\underline{ k }\dots}_{\underline{ l }\dots}
\end{aligned}
\end{equation}
$$
vlastnosti dědí od  [kovariantní derivace.](#D%20Kovariantn%C3%AD%20derivace%20(CD)) Navíc ultralokální: 
- $\nabla_{fa}A=f\nabla_{a}A;\, f\in \mathcal{F}M$
Speciálně: Podél křivky, kde $a=t$ je tečný vektor křivky tj. $\frac{\mathrm{D}z}{\mathrm{d}\tau}$,
$\nabla_{t}A=\frac{\nabla}{\mathrm{d}\tau}A$ akcent změny $A$ oproti změně $\tau$ parametru křivky.

_Vyjádření v souřadnicové bázi:_
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}A^{\underline{ k }\dots}_{\underline{ l }\dots}=\nabla_{a}A^{b\dots}_{c\dots}~({\mathrm{d}x^{a}})_{\underline{ m }}\cdots~ ({\partial_{x^{b}}})^{\underline{ k }}\cdots
\end{aligned}
\end{equation}
$$
_Alternativní značení:_
$$
\begin{equation}
\begin{aligned}
\nabla_{a}A^{b}_{c}\equiv A^{b}_{c;a}
\end{aligned}
\end{equation}
$$
_Příklad:_
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ a }}\delta^{\underline{ k }}_{\underline{ l }}=0\\
\text{neboť: } \delta^{\underline{ m }}_{\underline{ l }}v^{\underline{ l }}=v^{\underline{ k }}\, /\nabla_{\underline{ a }}\\
\nabla_{\underline{ a }}(\delta^{\underline{ k }}_{\underline{ l }}v^{\underline{ l }})
= (\cancelto{ 0 }{ \nabla_{\underline{ a }}\delta^{\underline{ k }}_{\underline{ l }} })v^{\underline{ l }}+\delta^{\underline{ k }}_{\underline{ l }}\nabla_{\overset{\underline{ a }}{_{_{1.}}}}v^{\underline{ l }}
=\nabla_{\underline{ a }}v^{\underline{ k }}
\end{aligned}
\end{equation}
$$
### D: Paralelní přenos
Pro křivku $z$, říkáme že se $A\in \mathcal{T}_{l}^{k}M$ paralelně přenáší po $z \iff$ 
$$
\begin{equation}
\begin{aligned}
\nabla_{t}A=0,\,\,\, \text{tzn. } \frac{ \nabla }{ \mathrm{d} \tau }A=0 ,
\end{aligned}
\end{equation}
$$
kde $t=\frac{\mathrm{D}z}{\mathrm{d}\tau}$.
## Příklady Kovariantních derivací
### Souřadnicová kovariantní derivace
Pro pevně zvolené souřadnice $x^{a}$ spojujeme $\partial$, která splňuje vlastnosti výše.
Navíc anihiluje bázové vektory/formy:
$$
\begin{equation}
\begin{aligned}
\partial \frac{ \partial  }{ \partial x^{a} } =0,\,\,\,\partial \mathrm{d}x^{a}=0
\end{aligned}
\end{equation}
$$
dovoluje zavést pojem _globální rovnoběžnosti_ v daných souřadnicích.
a je komutativní: $\partial_{\underline{ a }}\partial_{\underline{ b }}f-\partial_{\underline{ b }}\partial_{\underline{ a }}f=0$

### Frejmová kovariantní derivace
(bázová)
Volíme obecněji bázi vektorů/forem $e^{\underline{ a }}_{j},e^{j}_{\underline{ a }}$. Definujeme derivace $\cancel{\partial}$ tak aby anihilovala bázi.
$\cancel{ \partial }e_{j}=0=\cancel{ \partial }e^{j}$. Už neplatí komutace.

Komentář: Vidíme, že jich je spousta $\to$ prostor kovariantních derivací? případně hledat rozdíly.
### D: Konexe
Vztah 2 kov. derivací
Rozdílový faktor $\Gamma_{a}=\nabla_{a}-\tilde{\nabla}_{a};$  $a$ vektor.
Vlastnosti dědí po $\nabla_{a},\tilde{\nabla}_{a}$, Ale speciálně $\Gamma_{a}f=0$, protože obě působí na $\mathcal{F}M$ totožně.
Je to tedy tenzorová derivace anihilující fce, tedy [Pseudoderivace](Tenzorov%C3%A1%20pole#D%20Pseudoderivace%20$%5Cmathbf%7BM%7D$), a ta je určená akcí na vektorech.
Vyjádřena tenzorově:
$$
\begin{equation}
\begin{aligned}
{\Gamma_{a}}_{\underline{ n }}^{\underline{ m }}=a^{\underline{ k }}{\Gamma_{\underline{ k }}}^{\underline{ m }}_{\underline{ n }}
\end{aligned}
\end{equation}
$$
Na afinním prostoru [(CD)](#Kovariantn%C3%AD%20derivace) není definovaný součet, ale rozdíl ano. Rozdíl, který pak definuje směry, kterými jsou právě $\Gamma$:
![[afinní prostor CD.svg|400]]



__Jak zadat kovariantní derivaci?__
- vyberu počátek ($\tilde{\nabla}$)
- a průvodič ($\mathbf{\Gamma}$)
Typicky:
1) souřadnicová $x^{a}$
$\nabla =\partial+\Gamma$ (Christofelovy symboly jsou až složky této [pseudoderivace](Tenzorov%C3%A1%20pole#D%20Pseudoderivace%20$%20mathbf%7BM%7D$)) 
2) bázové $e_{j}$
$\nabla=\eth+\underbrace{ {\gamma} }_{ \text{spin koef.} }$

Komentář: Christofelovy symboly jsou tenzory pokud zůstáváme ve stejné bázi, pokud není $\Gamma$ rozkročena mezi dvěma souřadnicemi:
Zvolím si dvě souřadnice, každá určuje svou $\partial',\partial$. Pak i své vlastní $\Gamma',\Gamma$. V souřadnicích
$$
\begin{equation}
\begin{aligned}
\Gamma&=\Gamma^{\underline{ k }}_{\underline{ a b}}\to\Gamma^{k}_{ab}\\
\Gamma'&=\Gamma'^{\underline{ k }}_{\underline{ a b}}\to\Gamma'^{k'}_{a'b'}\\
\end{aligned}
\end{equation}
$$
jsou dva různé tenzory, a nelze mezi nimi přecházet souřadnicovou transformací:
![[Christofel.svg|400]]
Což lze vidět i výpočtem
$$
\begin{equation}
\begin{aligned}
0\overset{\text{def}}=\partial'\mathrm{~d}x^{a'}
&=\partial'({x^{a'}}_{,m}\mathrm{~d}x^{m})\\
&=\partial'({x^{a'}}_{,m})\mathrm{~d}x^{m}+{x^{a'}}_{,m}\partial'\mathrm{d}x^{m}\\
&={x^{a'}}_{,mn}\mathrm{~d}x^{m}\mathrm{~d}x^{n}+{x^{a'}}_{,m}(\partial-_{_{\Delta}}\!\!\gamma)\mathrm{~d}x^{m}\\
&=\underbrace{ {x^{a'}}_{,mn}\mathrm{~d}x^{m}\mathrm{~d}x^{n} }_{ \text{tenzor} }- \underbrace{{x^{a'}}_{,m} {}_{_{\Delta}}\!\!\gamma\mathrm{~d}x^{m} }_{ \text{konexe} }\\
&=({x^{a'}}_{,mn}-{}_{_{\Delta}}\!\!\gamma^{k}_{mn}{x^{a'}}_{,k})\mathrm{~d}x^{m}\mathrm{~d}x^{n}\overset!=0\quad\,\,\forall \mathrm{~d}x^{m}\mathrm{~d}x^{n}\\
\\
\implies {}_{_{\Delta}}\!\!\gamma^{k}_{mn}&={x^{a'}}_{,mn}{~x^{k}}_{,a'}
\end{aligned}
\end{equation}
$$
Z tohoto už víme jak se jednotlivé pseudoderivace transformují
$$
\begin{equation}
\begin{aligned}
\Gamma&=\Gamma'-{}_{_{\Delta}}\!\!\gamma\\
\Gamma^{a}_{bc}&=\Gamma'^{a}_{bc}-{}_{_{\Delta}}\!\!\gamma^{a}_{bc}\\
&=\Gamma'^{k'}_{m'n'}{~x^{a}}_{,k'}{~x^{m'}}_{,a}{~x^{n'}}_{,b}-{x^{k'}}_{,bc}{~x^{a}}_{,k'}
\end{aligned}
\end{equation}
$$
a podle předpokladu, kdy to není a ten samý tenzor, se netransformuje jako tenzor.

_Interpretace:_
$\nabla \frac{ \partial  }{ \partial x^{k} }=\cancelto{ 0 }{ \partial \frac{ \partial  }{ \partial x^{k} } }+\Gamma \cdot \frac{ \partial  }{ \partial x^{k} }$
Jak se mění souřadnicové vektory $\frac{ \partial  }{ \partial x^{i} }$ vzhledem k nové kovariantní derivaci.

### Lemma: Ultralokalita Cartanova vzorce pro CD
$$
\begin{equation}
\begin{aligned}
a^{\underline{ m }}\nabla_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\nabla_{\underline{ m }}a^{\underline{ n }}-[a,b]^{\underline{ n }}
\end{aligned}
\end{equation}
$$
je ultralokální objekt.
__Dk:__
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}b^{\underline{ n }} &= \partial_{\underline{ m }}b^{\underline{ n }} + \Gamma^{\underline{ n }}_{\underline{ mp }}\,b^{\underline{ p }}.\\
\\
a^{\underline{ m }}\nabla_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\nabla_{\underline{ m }}a^{\underline{ n }}
&= a^{\underline{ m }}(\partial_{\underline{ m }}b^{\underline{ n }}+\Gamma^{\underline{ n }}_{\underline{ mp }}b^{\underline{ p }})
   - b^{\underline{ m }}(\partial_{\underline{ m }}a^{\underline{ n }}+\Gamma^{\underline{ n }}_{\underline{ mp }}a^{\underline{ p }})\\
&= a^{\underline{ m }}\partial_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\partial_{\underline{ m }}a^{\underline{ n }}
   + a^{\underline{ m }}b^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }} - b^{\underline{ m }}a^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }}.\\ 
\\
\implies &[a,b]^{\underline{ n }} = a^{\underline{ m }}\partial_{\underline{ m }}b^{\underline{ n }} - b^{\underline{ m }}\partial_{\underline{ m }}a^{\underline{ n }}.\\
\end{aligned}
\end{equation}
$$
Pak  dohromady: 
$$
\begin{equation}
\begin{aligned}
a^{\underline{ m }}\nabla_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\nabla_{\underline{ m }}a^{\underline{ n }}-[a,b]^{\underline{ n }}
&= (a^{\underline{ m }}\partial_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\partial_{\underline{ m }}a^{\underline{ n }})
  + (a^{\underline{ m }}b^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }} - b^{\underline{ m }}a^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }})\\
&\,\quad - (a^{\underline{ m }}\partial_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\partial_{\underline{ m }}a^{\underline{ n }})\\[4pt]
&= a^{\underline{ m }}b^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }} - b^{\underline{ m }}a^{\underline{ p }}\Gamma^{\underline{ n }}_{\underline{ mp }}\\ 
\end{aligned}
\end{equation}
$$
Takže ve výsledku dostáváme pseudoderivaci, která je ultralokální ($\mathcal{F}M$-lineární):
$$
\begin{equation}
\begin{aligned}
a^{\underline{ m }}\nabla_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\nabla_{\underline{ m }}a^{\underline{ n }}-[a,b]^{\underline{ n }}
&= (a^{\underline{ m }}b^{\underline{ p }}-b^{\underline{ m }}a^{\underline{ p }})\,\Gamma^{\underline{ n }}_{\underline{ mp }}.
\end{aligned}
\end{equation}
$$
Objekt tedy můžeme reprezentovat jako tenzor, jako tzv. _Torze_
### D: Torze
$$
\begin{equation}
\begin{aligned}
T^{\underline{ n }}_{\underline{ mk }}a^{\underline{ m }}b^{\underline{ k }}:=a^{\underline{ m }}\nabla_{\underline{ m }}b^{\underline{ n }}-b^{\underline{ m }}\nabla_{\underline{ m }}a^{\underline{ n }}-[a;b]^{\underline{ n }}
\end{aligned}
\end{equation}
$$

_Interpretace:_
Nekomutativita na funkcích, kterou nezachytí Lieova závorka. Je to totiž míra nekomutativity kovariantních derivacích na fcích:
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}\nabla_{\underline{ n }}f-\nabla_{\underline{ n }}\nabla_{\underline{ m }}f=-T^{\underline{ k }}_{\underline{ mn }}\mathrm{~d}_{\underline{ k }}f
\end{aligned}
\end{equation}
$$
_Značení:_
$T=\text{Tor}[\nabla]$ 
_Vlastnosti:_
- $T[\partial]=0$
- Pro $n$-ádovou derivaci $\cancel{ \partial }:$ $T[\cancel{ \partial }]=t$
S použitím [Cartanova vzorce](Vn%C4%9Bj%C5%A1%C3%AD%20kalkulus#Cartanovy%20vzorce) a $\mathrm{~d}e^{k}=t^{k}$.
$$
\begin{equation}
\begin{aligned}
t^{\underline{ k }}_{\underline{ mn }} ~e^{\underline{ m }}_{a} ~e^{\underline{ n }}_{b}=-[e_{a};e_{b}]^{\underline{ k }}
\end{aligned}
\end{equation}
$$
čili nekomutativita bázových prvků.
_Life-hacks:_
Lze spočíst z antisymetrie indexů $\Gamma$. Pro dvě $\nabla=\tilde{\nabla}+\Gamma$
- $T^{\underline{ k }}_{\underline{ ab }}-\tilde{T}^{\underline{ k }}_{\underline{ ab }}=\Gamma^{\underline{ k }}_{\underline{ ab }}-\Gamma^{\underline{ k }}_{\underline{ ba }}=2\Gamma^{\underline{ k }}_{[\underline{ ab }]}$
__Dk:__ $\nabla_{a}\nabla_{b}f-\nabla_{b}\nabla_{a}f=\tilde{\nabla}_{a}\mathrm{~d}_{b}f+\Gamma^{k}_{ab}\mathrm{~d}_{k}f-\tilde{\nabla}_{b}\mathrm{~d}_{a}f-\Gamma^{k}_{ba}\mathrm{~d}_{k}f$.
$$
\begin{equation}
\begin{aligned}
-T^{k}_{ab}\mathrm{~d}_{k}f&=-\tilde{T}^{k}_{ab}\mathrm{~d}_{k}f-\left[\Gamma^{k}_{ab}-\Gamma^{k}_{ba}\right]\mathrm{~d}_{k}f\\
\end{aligned}
\end{equation}
$$
Nyní zvolme $\tilde{T}=\text{Tor}[\partial]=0$, pak zbude $\forall \mathrm{~d}_{k}f$: $T^{\underline{ k }}_{\underline{ ab }}=\Gamma^{\underline{ k }}_{[\underline{ ab }]}$.
V afinním prostoru je celá třída souřadnicových derivací, kdy je torze nulová.
![[afinní prostor - souřadnicové.svg|400]]


