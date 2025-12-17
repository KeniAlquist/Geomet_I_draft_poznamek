---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 21
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

# Integrovatelné hustoty
Integrace na varietách. Toto poslední téma je kulminace veškerých příprav, které jsme za semestry provedli. Pokud čtenář narazí na věci, které ho zmátnou, je srdečně vítám využít odkazy, kterých je v těchto posledních kapitolách hojně.

#### Motivace:
Obecně integrujeme tzv. *Extenzivní veličiny*, tj. takové veličiny rozložené v prostoru (na varietě) a aditivní v oblasti. Typicky $m_{\Omega}=\int_{\Omega}\mathrm{d}m$, kde $\mathrm{d}m$ budou hladké míry, kteréžto budou středem našeho zájmu z pohledu diferenciální geometrie.
Mají skalární charakter, aditivita vyplývá z ultralokality. Takové míry tvoří 1-dim vektorový prostor, a jsou si tedy ekvivalentní až na násobek konečným reálným číslem $\mathrm{d}m=\mu \mathrm{d}V$. Můžu se tedy bavit i o podílu hodnot $\mu=\frac{\mathrm{d}m}{\mathrm{d}V}$. 
Takže si stačí zvolit referenční hustotu a ostatní označit třeba právě tímto násobkem: $\mathrm{d}\rho \equiv\rho \mathrm{d}V$.
##### Referenční souřadnice:
$(U,x)$, s $U$ oblast s $x$. Oblast $\Omega \subset U$, tj. můžu oblast pokrýt stejnými souřadnicemi. Pak můžeme definovat souřadnicový objem $\Delta^{m}x=\int_{\Omega}\mathrm{d}^{m}x$, objem měřený souřadnicovými buňkami. 
Do jiných souřadnicových objemů se můžu transformovat pomocí Jakobiánu:
$$
\begin{equation}
\begin{aligned}
\int_{\Omega}\mathrm{d}^{m}x=\int_{\Omega}\left|\det \frac{ \partial x^{k} }{ \partial x^{l'} } \right|\mathrm{d}^{m}x'
\end{aligned}
\end{equation}
$$
čili $\mathrm{d}^{m}x=\left|\det \frac{ \partial x^{k} }{ \partial x^{l'} } \right|\mathrm{d}^{m}x'$, jak se mění měřící objem.
Obecnou hustotu pak pomocí reálný koeficientu $\alpha_{x}\equiv da_{x}$ vztaženému k referenčním $x$:
$$
\begin{equation}
\begin{aligned}
\mathrm{d}a=\alpha_{x} \mathrm{d}^{m}x\equiv da_{x}\mathrm{d}^{m}x
\end{aligned}
\end{equation}
$$
Z toho plynou transformační vlastnosti
$$
\begin{equation}
\begin{aligned}
\alpha_{x'}=\frac{\mathrm{d}a}{\mathrm{d}^{m}x'}=\left|\det \frac{ \partial x^{k} }{ \partial x^{l'} } \right| \frac{\mathrm{d}a}{\mathrm{d}^{m}x^{l'}}=\left|\det \frac{ \partial x^{k} }{ \partial x^{l'} } \right|\alpha_{x}.
\end{aligned}
\end{equation}
$$
Čili pak je jedno, ve kterých referencích měřím, pokud jsou koeficienty patří souřadnicím:
$$
\begin{equation}
\begin{aligned}
\int_{\Omega}\mathrm{d}a=\int_{\Omega}\alpha_{x}\mathrm{d}x=\int_{\Omega}\alpha_{x'}\mathrm{d}x'
\end{aligned}
\end{equation}
$$
Paradoxně by se zdálo, že $\left|\det \frac{ \partial x^{k} }{ \partial x^{l'} } \right|$ potřebuje informaci nejen v jednom bodě, tj. ve sporu s ultralokalitou, ale  zde postačí informace z tečného prostoru v tom bodě. Souřadnicová buňka má tak tvar "napnutý" na souřadnicových vektorech $\frac{ \partial  }{ \partial x^{k} }$ (resp. bázových vektorech $\mathbf{e}_{k}$)
$$
\begin{equation}
\begin{aligned}
\mathrm{d}a\left[ \frac{ \partial  }{ \partial x^{k} }  \right]\equiv da_{x}
\end{aligned}
\end{equation}
$$
množství kvantity "napnuté" na bázi. A dále v závislosti na libovolné bázi
$$
\begin{equation}
\begin{aligned}
\mathrm{d}a[\mathbf{e}_{k'}]\equiv \mathrm{d}a[A_{k'}^{l}\mathbf{e}_{l}]=\left|\det A^{l}_{k'} \right|\mathrm{d}a[\mathbf{e}_{l}].
\end{aligned}
\end{equation}
$$
###### Značení:
Ve fyzikálním kontextu použijeme $\mathrm{d}m,\mathrm{d}q$
Pro odlehčení značení $\mathfrak{a}, \mathfrak{b}, \mathfrak{m}$, případně $\tilde{a},\tilde{b},\tilde{m}$


### D: Integrovatelná hustota v bodě
nad $\mathbf{T}_{\mathscr{x}}M$ je
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}:\text{báze}\mathbf{T}_{\mathscr{x}}M&\to \mathbb{R}\\
\mathbf{e}_{k}&\mapsto \mathfrak{a}[\mathbf{e}_{k}],
\end{aligned}
\end{equation}
$$
splňující transformační vlastnost
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}[A^{k}_{l'}\mathbf{e_{k}}]=\left|\det A^{k}_{l'}\right|\mathfrak{a}[\mathbf{e}_{l'}]
\end{aligned}
\end{equation}
$$
(Jakési zobecněné tenzory, reprezentace transformací na objektech lehce obecnější než tenzor)

Prostor hustot v $\mathscr{x}$ ozn. $\mathbf{H}_{\mathscr{x}}M$, a má přirozenou vektorovou strukturu:
- $(\mathfrak{a}+\mathfrak{b})[\mathbf{e}_{k}]=\mathfrak{a}[\mathbf{e}_{k}]+\mathfrak{b}[\mathbf{e}_{k}]$
- $(r\mathfrak{a})[\mathbf{e}_{k}]=r\mathfrak{a}[\mathbf{e}_{k}]$
Prostor hustot je 1-dim:
- $\frac{\mathfrak{a}}{\mathfrak{b}}=\frac{\mathfrak{a}[\mathbf{e}_{k}]}{\mathfrak{b}[\mathbf{e}_{k}]}$, pro libovolnou bázi (pro nenulové hustoty $\mathfrak{b}$)

(Speciálně při zobecnění do pole se prostor popisuje z pohledu funkčního a tedy to reflektuje značení $\mathcal{\tilde{F}}M$)

### D: Bázové hustoty
Bázi $\mathbf{e}_{j}\mapsto \mathfrak{e}$, tak že $\mathfrak{e}[\mathbf{e_{j}}]=1$. A tedy v souřadnicích bychom označili, jak známe: $\frac{ \partial  }{ \partial x^{j} }\mapsto \mathrm{d}^{m}x$, tak že $\mathrm{d}^{m}x\left[ \frac{ \partial  }{ \partial x^{j} } \right]=1$. Nebo obecně
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}=\underline{ \mathfrak{a}_{x} }_{\mathbb{R}}\mathrm{d}^{m}x=\underline{ \mathfrak{a}_{e} }_{\mathbb{R}}\mathfrak{e}
\end{aligned}
\end{equation}
$$
Konzistentně taky transformace $\mathrm{d}^{m}x'=\mathrm{d}^{m}x'\left[ \frac{ \partial  }{ \partial x^{j} } \right]~~\mathrm{d}^{m}x=\mathrm{d}^{m}x'\left[ \frac{ \partial x^{k'} }{ \partial x^{j} }\frac{ \partial  }{ \partial x^{k'} } \right]\mathrm{d}^{m}=\left|\det \frac{ \partial x^{k'} }{ \partial x^{j} }\right|\underbrace{ \mathrm{d}^{m}x\left[ \frac{ \partial  }{ \partial x^{j} } \right] }_{ 1 }\mathrm{d}^{m}x$.

### Pozn: Integrování hustot
$\Omega$ Pokryté mapou $U,\mathscr{x}$
$$
\begin{equation}
\begin{aligned}
\int_{\Omega} \mathfrak{a}=\int_{\Omega}\mathfrak{a}\left[ \frac{ \partial  }{ \partial x^{j} }  \right]\mathrm{d}^{m}x
\end{aligned}
\end{equation}
$$
Pokud $\Omega$ je pokryto více mapami, pak rozsekáme na menší oblasti a aditivně posčítáme příspěvky každé části. (hladkým rozsekáním, neostře *Rozkladem jednotky*)

### D: Hustoty obecné váhy $w$
$\mathfrak{a}$ je hustota obecné váhy $w$ z $\mathbf{H}^{w}_{\mathscr{x}}M$. (Budeme pracovat pouze s váhami konstantními na celé varietě)
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}: \text{báze }\mathbf{T}_{\mathscr{x}}M\to \mathbb{R}
\end{aligned}
\end{equation}
$$
tak, že $\mathfrak{a}[A^{k}_{l'}\mathbf{e}_{k}]=\left|\det A^{k}_{l'}\right|^{w}\mathfrak{a}[\mathbf{e}_{k}]$
Opět se jedná o 1-dim vektorový prostor. Umožňuje definovat součin hustot: $\mathbf{H}^{a}_{\mathscr{x}}M\times\mathbf{H}^{b}_{\mathscr{x}}M\to\mathbf{H}^{a+b}_{\mathscr{x}}M$, neboli $(\mathfrak{a}\mathfrak{b})[\mathbf{e}_{j}]=\mathfrak{a}[\mathbf{e}_{j}]\mathfrak{b}[\mathbf{e}_{j}]$. A transformační vlastnosti jsou naprosto analogické.
A p-mocninovou analogii: $\mathbf{H}^{w}_{\mathscr{x}}M\to\mathbf{H}^{pw}_{\mathscr{x}}M$, neboli $(\mathfrak{a}^{p})[\mathbf{e}_{j}]=(\mathfrak{a}[\mathbf{e}_{j}])^{p}$. (Mohou nastat problémy pokud by hustoty byly záporné).

### D: Absolutní hodnota
Operace
$$
\begin{equation}
\begin{aligned}
|\cdot|: \mathbf{H}_{\mathscr{x}}M &\to \mathbf{H}^{+}_{\mathscr{x}}M\\
\mathfrak{a}&\mapsto|\mathfrak{a}|
\end{aligned}
\end{equation}
$$
Tedy $|\mathfrak{a}|[\mathbf{e}_{j}]=|\mathfrak{a}[\mathbf{e}_{j}]|$.
Rozlišíme tak kladné a záporné hustoty.
$$
\begin{equation}
\begin{aligned}
\mathbf{H}^{\pm}_{\mathscr{x}}M\ni \iff \mathfrak{a}[\mathbf{e}_{j}]\gtrless 0
\end{aligned}
\end{equation}
$$
![[AbsMapDens.svg|200]]
### D: Tenzorové hustoty
Další zobecnění se nabízí Tenzorové hustoty.
$$
\begin{equation}
\begin{aligned}
{\mathbf{\tilde{T}}^{w}}^{k}_{l}M = {\mathbf{T}}_{l}^{k}\otimes \mathbf{H}^{w}M
\end{aligned}
\end{equation}
$$
Například: $\mathfrak{a}^{\underline{ m }}=a^{\underline{ m }}\mathfrak{m}$, takže je násobení tou hustotou.

### D: Geometrický objem
Daný metrikou, krychle ve smyslu [metriky](Metrick%C3%A1%20Struktura#D%20Z%C3%A1kladn%C3%AD%20Metrika) $g$. Určuje ortonormální bázi $\mathbf{e}_{j}$.
$$
\begin{equation}
\begin{aligned}
\mathrm{d}V[\mathbf{e}_{j}]=1
\end{aligned}
\end{equation}
$$
nezávisle na $\mathbf{e}_{j}$ Jakobi $\pm{1}$.

V obecné neortonormální bázi $\mathbf{e}_{k}$ tedy:  $\mathrm{d}V[\mathbf{e}_{k}]=|\det g_{kl}|^{1/2}$, $g_{kl}=\mathbf{e}_{k}\cdot g\cdot \mathbf{e_{l}}$.
$\det g_{kl}=\det(A^{k'}_{k}A^{l'}_{l}g_{k'l'})=(\det A^{k'}_{k})^{2}\underbrace{ \det g_{k'l'} }_{ \text{sign} g }$ (sign dál nepíšeme) a tedy po odmocnění dostáváme absolutní hodnotu $|\det g_{kl}|^{1/2}=|\det A^{k'}_{k}|$, kterou můžeme dosadit do [transformace bázové hustoty](#D%20B%C3%A1zov%C3%A9%20hustoty). 
(Časté značení $\mathrm{d}l,\mathrm{d}S,\mathrm{d}V,\mathrm{d}\Omega$ a pro metriky $\text{g}^{1/2}$, příp. $\mathscr{q}^{1/2}$)

### D: Determinant metriky
Dříve jsme použili determinant metriky, to můžeme obecněji zavést pro tenzory stupně 2, kterým přiřadí [integrální hustotu váhy 2](#D%20Hustoty%20obecn%C3%A9%20v%C3%A1hy%20$w$).
$$
\begin{equation}
\begin{aligned}
\text{Det}:\mathbf{T}_{2}^{0}M&\to \mathbf{H}^{2}M\\
g&\mapsto \text{Det}g
\end{aligned}
\end{equation}
$$
Tedy $(\text{Det}g)[\mathbf{e}_{k}]=\det g_{kl}$. Což označujeme $\text{g}\equiv|\text{Det}g|$.

## Vztah $\mathbf{H}_{}M$ a $\Lambda^{m}M$
[[Vnější kalkulus|Totálně antisymetrické formy]] (TASF) $\omega \in\Lambda^{m} M$ s bází ${\varepsilon}=e^{1}\wedge\dots \wedge e^{m}$. $\omega=\omega_{1\dots m}\varepsilon=\omega_{1'\dots m'}\varepsilon'$. Pro ně víme, že $\omega_{1'\dots m'}=(\det A^{k}_{l'})\omega_{1\dots m}$, kde jsou $A$ transformace antisym. báze $\varepsilon^{k}=A^{k}_{l'}\varepsilon^{l'}$. Vidíme, že jsou velmi podobné transformacemi integrálním hustotám, avšak jsou citlivější na znaménko. Nabízí se definovat absolutní hodnotu formy.
### D: Absolutní hodnota TASF
$$
\begin{equation}
\begin{aligned}
|\cdot|: \Lambda^{m}M\to \mathbf{H}M\\
|\omega|[\varepsilon_{j}]=|\omega_{1\dots m}|
\end{aligned}
\end{equation}
$$

![[AbsMapTASF.svg|270]]
Komentář: U forem nevíme, které jsou hladké a které záporné bez dodatečného určení. U hustot to víme. Podle obrázku intuitivně vidíme, že prostory $\Lambda M$ a $\mathbf{H}_{}M$ jsou podobně velké. A protože to je jen absolutní hodnota, tak mají stejnou škálu.

Orientace dvěma způsoby:
- Báze zvolíme jednu třídu orientované báze jako kladnou.
- TASF pokud by při stejně zvolené bázi měly stejné znaménko komponenty. (znaménko faktoru úměrnosti) a volíme kladnou.
- zvoleným isomorfizmem $\Lambda^{m}M$ a $\mathbf{H}M$:
![[IsoMapDensTASF.svg|270]]
### D: Tenzor orientace
Máme zvolenou orientaci bází, TASF a $\iota_{+}$. Pak pro libovolnou $\omega \in \Lambda^{m}M$
$$
\begin{equation}
\begin{aligned}
\tilde{\varepsilon}&=\omega(\iota_{+}\omega)^{-1}\in \Lambda^{m}\otimes \mathbf{H}^{-1}M,\\
\tilde{\varepsilon}^{-1}&=\omega^{-1}(\iota_{+}\omega)\in \mathbf{T}^{[m]}_{0}\otimes \mathbf{H}M,
\end{aligned}
\end{equation}
$$
kde je $\omega^{-1}$  [inverze TAFS](Metrick%C3%A1%20Struktura#Tot%C3%A1ln%C4%9B%20antisymetrick%C3%A9%20tenzory#inverze). 
###### Poznámky:
Díky tomu lze $\iota_{+},\iota_{-}$ psát pomocí $\tilde{\varepsilon}^{-1},\tilde{\varepsilon}$. Oběma směry:
- $\iota_{+}:\Lambda^{m}M\to \mathbf{H}M\qquad\alpha\mapsto\tilde{\varepsilon}^{-1}\bullet\alpha$
- $\iota_{+}^{-1}:\mathbf{H}M\to\Lambda^{m}M\qquad\mathfrak{a}\mapsto \mathfrak{a}\tilde{\varepsilon}$
Pro positivní $\omega$: $\iota_{+}\omega=|\omega|$ z čehož vyplývá, že $\tilde{\varepsilon}=\omega|\omega|^{-1}$. 
V komponentách vyjádřené tenzory orientace:
- V positivně orientovaný souřadnicích (tj. v positivně orientované bázi) $\mathrm{d}^{m}x=|\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{m}|$, dostáváme 
$$
\begin{equation}
\begin{aligned}
\tilde{\varepsilon}&=(\mathrm{d}^{m}x)^{-1}~~\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{m}\\
\tilde{\varepsilon}^{-1}&=(\mathrm{d}^{m}x)~~\partial_{x^{1}}\wedge\dots \wedge \partial_{x^{m}}
\end{aligned}
\end{equation}
$$
a v takovéto bázi je komponenta Tenzoru orientace, též „Levi-Civitův symbol”, triviálně ${\tilde{\varepsilon}_{\mathscr{x}}}_{1\dots m}=1={\tilde{\varepsilon}^{-1}_{\mathscr{x}}}^{1\dots m}$ (znaménkové změny díky permutaci se kompenzují). Pro negativně orientovanou bázi budou komponenty $-1$. 
Je dobře vidět, jak jednoduše $\tilde{\varepsilon}$ působí, kde odebírá, resp. přidává $\mathrm{d}x\wedge\dots \wedge\mathrm{d}x$ část a přidává, resp. odebírá $\mathrm{d}^{m}x$. Můžeme tak ztotožnit přímo komponenty $\alpha_{1\dots m}=\mathfrak{a}_{x}$:
$$
\begin{equation}
\begin{aligned}
\alpha &=\alpha_{1\dots m}~~\mathrm{d}^{1}x\wedge\dots \wedge \mathrm{d}x^{m}\\
\iota_{+} \to \mathfrak{a}&=\mathfrak{a}_{x}~~\mathrm{d}^{m}x
\end{aligned}
\end{equation}
$$

### D: Orientovatelnost variety
Pokud orientace lze volit spojitě na celé varietě stejně. (viz. Möbiuv pásek). Existuje hladký globální tenzor orientace $\tilde{\varepsilon}$. 

Na orientovatelných varietách lze integrovat TASFormy:
$$
\begin{equation}
\begin{aligned}
\int_{\Omega}\alpha=\int_{\Omega}\mathfrak{a},\qquad\mathfrak{a}=\tilde{\varepsilon}^{-1}\bullet\alpha\\
\end{aligned}
\end{equation}
$$

### D: Derivace hustot
Rozšíření [tenzorové derivace](Tenzorov%C3%A1%20pole#D%20$%20mathbf%7BD%7D$%20Tenzorov%C3%A1%20derivace) na pole [integrovatelných hustot](#D%20Tenzorov%C3%A9%20hustoty) $\mathbf{H}_{}M$, respektive i na hustoty obecné váhy. Komutuje s $\iota_{\pm}$, (není třeba globální orientovatelnost)
$$
\begin{equation}
\begin{aligned}
\mathbf{D}(\iota_{+}\alpha)=\iota_{+}(\mathbf{D}\alpha)
\end{aligned}
\end{equation}
$$
a zároveň, jelikož $\mathfrak{a}=\iota_{+}\alpha=\tilde{\varepsilon}^{-1}\bullet\alpha$  a  $\alpha=({\iota_{+}})^{-1}\mathfrak{a}=\mathfrak{a}\tilde{\varepsilon}^{-1}$, tak
$$
\begin{equation}
\begin{aligned}
\tilde{\varepsilon}\mathbf{D}\mathfrak{a}=\mathbf{D}(\tilde{\varepsilon}\mathfrak{a})
\end{aligned}
\end{equation}
$$
$\iff$ Podmínka $\mathbf{D}\tilde{\varepsilon}=0$. Je konstantní vůči derivování. a nezávisí na konkrétní volbě orientace ($\pm{1}$)
Pak již můžeme psát přímo derivace tenzorové hustoty.
$$
\begin{equation}
\begin{aligned}
\mathbf{D}\mathfrak{a}=\underbrace{ \mathfrak{a}\alpha^{-1} }_{ \tilde{\varepsilon}^{-1} }\bullet \mathfrak{D}\alpha
\end{aligned}
\end{equation}
$$
##### Poznámky:
- Pro $w\in \mathbb{R}$ platí
$$
\begin{equation}
\begin{aligned}
\mathbf{D}\mathfrak{m}^{w}=w\mathfrak{m}^{w-1}~~\mathbf{D}\mathfrak{m}
\end{aligned}
\end{equation}
$$

- Akce [pseudoderivace](Tenzorov%C3%A1%20pole#D%20Pseudoderivace%20$%20mathbf%7BM%7D$) na $\mathbf{H}_{}M$.
$$
\begin{equation}
\begin{aligned}
\mathbf{M}\tilde{\varepsilon}=0,\qquad\mathbf{M(\iota_{+}\alpha)}=\iota_{+}(\mathbf{M}\alpha),
\end{aligned}
\end{equation}
$$
ale [pseudoderivace na TASF](Metrick%C3%A1%20Struktura#Akce%20pseudoderivace) se zjednoduší na přenásobení stopou tenzoru $\mathbf{M}$. Takže pro hustoty:
$$
\begin{equation}
\begin{aligned}
\mathbf{M}\mathfrak{m}&=-M^{\underline{ n }}_{\underline{ n }}\mathfrak{m},\quad\quad \mathfrak{m} \text{ váhy }1\\
\mathbf{M}\mathfrak{a}&=-w~M^{\underline{ n }}_{\underline{ n }}\mathfrak{a},\quad\quad \mathfrak{a} \text{ váhy }w
\end{aligned}
\end{equation}
$$ 
^jzzs1h
- Tyto vlastnosti již určují [kovariantní derivaci](Kovariantn%C3%AD%20Derivace#D%20Kovariantn%C3%AD%20derivace%20(CD)) na $\mathbf{H}_{}M$
$\nabla \tilde{\varepsilon}=0$,
$\nabla=\bar{\nabla}+\Gamma$.
Působení na tenzorovou hustotu $\tilde{A}$ jako
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}\tilde{A}^{\underline{ k }\dots}_{\underline{ l }\dots}=\bar{\nabla}\tilde{A}^{\underline{ k }\dots}_{\underline{ l }\dots}+\Gamma^{\underline{ k }}_{\underline{ mn }}\tilde{A}^{\underline{ n }}_{\underline{ l }}+\dots-\Gamma^{\underline{ n }}_{\underline{ ml }}\tilde{A}^{\underline{ k }}_{\underline{ n }}-\dots-w\Gamma_{\underline{ mn }}^{\underline{ n }}\tilde{A}^{\underline{ k }}_{\underline{ l }}
\end{aligned}
\end{equation}
$$
a přibude stopa z $\Gamma$.

- [Ricciho identity](K%C5%99ivost#D%20Ricciho%20identity) jelikož, [operátor křivosti](K%C5%99ivost#Lemma%20Oper%C3%A1tor%20k%C5%99ivost%20jako%20pseudoderivace) je pseudoderivace a 
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}~\mathfrak{m}&=-w{{R_{ab}}^{\underline{ n }}}{}_{\underline{ n }}~\mathfrak{m}=-w~a^{\underline{ k }}b^{\underline{ l }}~\text{Tr}[\mathbf{R}_{\underline{ kl }}]~\mathfrak{m}\\
\mathbf{R}_{\underline{ kl }}~\mathfrak{m}&=-w~\text{Tr}[\mathbf{R}_{\underline{ kl }}]~\mathfrak{m} = \left[ \nabla_\underline{ k } \nabla_\underline{ l } - \nabla_\underline{ l } \nabla_\underline{ k } - T_{\underline{ kl }}^{\underline{ n }}\nabla_{\underline{ n }}  \right]\mathfrak{m}
\end{aligned}
\end{equation}
$$
Kdysi byla [formulována podmínka integrability (CD) na TASF](K%C5%99ivost#Veta%20Integrabilita%20$%20nabla$%20na%20$%20mathcal%7BA%7D%20%7Bd%7DM$), která se jednoduše přeloží na hustoty.
$$
\begin{equation}
\begin{aligned}
\mathrm{Tr}\mathbf{R}=0 &\iff \exists \omega \in \mathcal{A}^{d}M,\qquad\nabla \omega=0\\
&\iff \exists \mathfrak{a}, \nabla \mathfrak{a}=0 \\
\end{aligned}
\end{equation}
$$
Tj. [Stopa Riemannova tenzoru](K%C5%99ivost#Stopy%20Riemannova%20tenzoru) je nulová když je [[|objemová hustota]] konstantní vůči kovariantní derivaci. (Konkrétně [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace) je metrická, tedy zachovává i metrický objemový element)





