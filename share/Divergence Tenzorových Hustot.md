---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 22
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

# Divergence tenzorových hustot

## Dualita TAS forem a TAS tenzorových hustot
Vztah [antisymetrických p-forem](Vnější%20kalkulus) a duální struktuře antisymetrických tenzorových hustot.
$$
\begin{equation}
\begin{aligned}
\Lambda^{p}M \quad\quad\mathcal{A}^{p}M\quad\quad &\text{p-formy}\\
\mathbf{H}\otimes \mathbf{T}^{[p]}_{0}M=\Lambda^{\#p}M \quad\quad\mathcal{A}^{\#p}M\quad\quad &\text{p-AS tenz. hustoty}\\
\end{aligned}
\end{equation}
$$
### D: Hustotní duál
$$
\begin{equation}
\begin{aligned}
\# : \Lambda^{n}M \longleftrightarrow \Lambda^{\#\hat{n}}M,\quad\quad n+\hat{n}=m
\end{aligned}
\end{equation}
$$
se dimenze sčítají podobně jako u hoedgeova duálu. Nyní to jsou však dvě operace.
$$
\begin{equation}
\begin{aligned}
\#: \Lambda^{n}M \to \Lambda^{\#\hat{n}}M,\quad\quad\alpha \mapsto\#\alpha=\tilde{\varepsilon}^{-1}\bullet\alpha=\mathfrak{a}
\end{aligned}
\end{equation}
$$
^q9zd3w

(část horních indexů tenzoru orientace se zkrátí s dolními index formy $\alpha$ a zbude hustota)
Což vidíme v indexech $\mathfrak{a}^{a_{1}\dots a_{\hat{n}}}=\frac{1}{n!}{\tilde{\varepsilon}^{-1}}^{a_{1}\dots a_{\hat{n}}k_{1}\dots k_{n}}\alpha_{k_{1}\dots k_{n}}$.
$$
\begin{equation}
\begin{aligned}
\#:\Lambda^{\#\hat{n}}M\to\Lambda^{n}M,\quad\quad\mathfrak{a}\mapsto \alpha =\#\mathfrak{a}=\mathfrak{a}\bullet\tilde{\varepsilon}
\end{aligned}
\end{equation}
$$
(Úženo z opačné strany. Používáme konvenci řazení $\text{norm-tečn}$ složky, formy přísluší tečn.)
Pro ilustraci v indexech $\alpha_{a_{1}\dots a_{n}}=\frac{1}{n!}\mathfrak{a}^{k_{1}\dots k_{\hat{n}}}\tilde{\varepsilon}_{k_{1}\dots k_{\hat{n}}a_{1}\dots a_{n}}$.

**Platí relace duality:** $\#\#\alpha=\alpha$ a $\#\#\mathfrak{a}=\mathfrak{a}$.
Na rozdíl od hoedgeova duálu zde nevystupují žádná znaménka, kvůli přirozenosti definice $\#$.

**speciální případy:** 
$\#\omega=\iota_{+}\omega$ pro $\omega \in\Lambda^{m}M$.
$\#\mathfrak{m}={\iota_{+}}^{-1}\mathfrak{m}$ pro $\mathfrak{m} \in\mathbf{H}_{}M$.
$\#1=\tilde{\varepsilon}^{-1}$ pro $1\in\Lambda^{0}M$
$\# \tilde{\varepsilon}^{-1}=1$ pro $\tilde{\varepsilon}^{-1}\in\Lambda^{\#m}M$
$\#(a\mathfrak{m})=a\cdot(\#\mathfrak{m})$ pro $a\in \mathbf{T}_{}M\,\mathfrak{m}\in \mathbf{H}_{}M$
$\#(\mathfrak{a}\wedge a)=a\cdot(\#\mathfrak{a})$ pro $a\in \mathbf{T}_{}M\,\mathfrak{a}\in\Lambda^{\#\hat{n}}M$
poslední řádek je obecnější případ předchozího. Indexy je naznačíme

**Příklady na rozmyšlení:**
- $\#(\mathfrak{a}\wedge a)=\frac{1}{(\hat{n}+1)!}(\mathfrak{a}\wedge a)^{--a}\tilde{\varepsilon}_{--a,,,,}=\frac{\hat{n}+1}{(\hat{n}+1)!}\mathfrak{a}^{--}a^{a}\tilde{\varepsilon}_{--a,,,,}=(\#\mathfrak{a})_{a,,,,}a^{a}=(a\cdot\#\mathfrak{a})_{,,,,}$
- $\underbrace{ \mathfrak{a} }_{ \hat{n} }\bullet\underbrace{ (\#\mathfrak{b}) }_{ \hat{n} }=(-1)^{n\hat{n}}~\underbrace{ (\#\mathfrak{a}) }_{ n }\bullet\underbrace{ \mathfrak{b} }_{ n }$
- $\alpha\bullet(\#\beta)=(-1)^{n\hat{n}}~(\#\alpha)\bullet\beta$
Pokud máme metriku můžeme dát do vztahu s hoedgeovým duálem.
- $*\alpha=\#~\sharp\text{g}^{1/2}\alpha$
- $\#\mathfrak{a}=*\flat\text{g}^{-\frac{1}{2}}\mathfrak{a}$
![[DualniVztahySMetrikou.svg|center]]
### Lemma: Vztah komponent TAS forem a TAS tenzorových hustot
Ve standardním rozpisu komponent AS forem, kde $\alpha \in\Lambda^{n}M$  $\mathfrak{a}\in\Lambda^{\#\hat{n}}M$  $\alpha=\#\mathfrak{a}$
$$
\begin{equation}
\begin{aligned}
\alpha=\sum_{k_{1}<\dots<k_{n}}\alpha_{k_{1}\dots  k_{n}}\mathrm{d}x^{k_{1}}\wedge\dots \wedge \mathrm{d}x^{k_{n}}\equiv\sum_{K}\alpha_{K}~\mathrm{d}x^{\wedge K},
\end{aligned}
\end{equation}
$$
kde uspořádané indexy přeznačíme $k_{1}<\dots<k_{n}\equiv K$.
$$
\begin{equation}
\begin{aligned}
\mathfrak{a}= \sum_{l_{1}<\dots<l_{\hat{n}}}\mathfrak{a}^{l_{1}\dots l_{\hat{n}}}_{x}~\partial_{x^{l_{1}}}\dots \partial_{x^{l_{\hat{n}}}}\equiv \sum_{L}\mathfrak{a}_{x}^{L}\partial_{x}^{\wedge L},
\end{aligned}
\end{equation}
$$
kde navíc musíme označit, vůči jaké bázi se hustota vyčísluje.

**Vztah komponent:**
$$
\begin{equation}
\begin{aligned}
\alpha_{K}=(\mathfrak{a}\bullet\tilde{\varepsilon})_{K}=\sum_{L}\mathfrak{a}_{x}^{L}\tilde{\varepsilon}_{xLK}
\end{aligned}
\end{equation}
$$
Ovšem pozor pro $K$ existuje pouze jedna kombinace z doplňkových indexů $L$ ozn. $\hat{K}$, čili 
$$
\begin{equation}
\begin{aligned}
=\mathfrak{a}_{x}^{\hat{K}} \tilde{\varepsilon}_{xK\hat{K}}.
\end{aligned}
\end{equation}
$$
komponenty si odpovídají až na znaménko [tenzoru orientace](Integrovateln%C3%A9%20hustoty#D%20Tenzor%20orientace). 
### D: Divergence
Jako duál [vnější derivace](Vn%C4%9Bj%C5%A1%C3%AD%20kalkulus#D%20Vnější%20derivace%20(už%20nutně%20na%20formách)) z antisymetrických forem. $\mathrm{~d}:\mathcal{A}^{p}M\to\mathcal{A}^{p+1}M$.
$$
\begin{equation}
\begin{aligned}
\text{div}:\mathcal{A}^{\#\hat{n}+1}M\to \mathcal{A}^{\#\hat{n}}_{}M
\end{aligned}
\end{equation}
$$
$\text{div}=\#\mathrm{d}\#$.
### Lemma: Div Div
$$
\begin{equation}
\begin{aligned}
\text{div}~~\text{div}=0
\end{aligned}
\end{equation}
$$
Analogicky k $\mathrm{d}\mathrm{d}=0$.
### Věta: Podstata Div
$$
\begin{equation}
\begin{aligned}
(\text{div}\mathfrak{a})^{a_{1}\dots a_{\hat{n}}}=\nabla_{\underline{ b }}\mathfrak{a}^{a_{1}\dots a_{\hat{n}}\underline{ b }}
\end{aligned}
\end{equation}
$$
Pro libovolné beztorzní $\nabla$. (S [torzí](Kovariantn%C3%AD%20Derivace#D%20Torze) by obsahovala [členy navíc v analogii](U%C5%BEite%C4%8Dn%C3%A9%20Vztahy%20Derivac%C3%AD#Věta%20$\mathrm{d}$%20a%20$\nabla$).)

**DK:**
využijeme lemmatu [nezávislosti na beztorzní CD](#Lemma%20Nezávislost%20Div).
$$
\begin{equation}
\begin{aligned}
(\text{div}\mathfrak{a})^{\underline{ a_{1} }\dots\underline{ a_{\hat{n}} }}&=(\# \nabla \wedge\#\mathfrak{a})^{\underline{ a_{1} }\dots\underline{ a_{\hat{n}} }}=\frac{1}{n!} \tilde{\varepsilon}^{-1\underline{ a_{1} }\dots \underline{ a_{1} }\underline{ k_{1} }\dots\underline{ k_{n} }}(\nabla \wedge\#\mathfrak{a})_{\underline{ k_{1} }\dots\underline{ k_{n} }}\\
&=\frac{n}{n!} \tilde{\varepsilon}^{-1\underline{ a_{1} }\dots\underline{ a_{\hat{n}} }l\dots l}\nabla_{l}(\#\mathfrak{a})_{\underline{ k_{2} }\dots \underline{ k_{n }}}\\
&=\frac{1}{(n-1)!} \frac{1}{(\hat{n}-1)!} \tilde{\varepsilon}^{-1\underline{ a_{1} }\dots \underline{ a_{\hat{n}} }\underline{ k_{1} }\dots\underline{ k_{n} }}\nabla_{\underline{ k_{1} }}(\mathfrak{a}^{\underline{ b_{1} }\dots \underline{ b_{\hat{n}+1} }} \tilde{\varepsilon}_{\underline{ b_{1} }\dots\underline{ b_{\hat{n}+1} }\underline{ k_{2} }\dots \underline{ k_{n} }})\\
&=\frac{1}{(n-1)!} \frac{1}{(\hat{n}-1)!} \tilde{\varepsilon}^{-1\underline{ a_{1} }\dots \underline{ a_{\hat{n}} }\underline{ k_{1} }\dots\underline{ k_{n} }}\nabla_{\underline{ k_{1} }}(\mathfrak{a}^{\underline{ b_{1} }\dots \underline{ b_{\hat{n}+1} }}) \tilde{\varepsilon}_{\underline{ b_{1} }\dots\underline{ b_{\hat{n}+1} }\underline{ k_{2} }\dots \underline{ k_{n} }}\\
&=\underbrace{\frac{m!}{(n-1)!} \frac{1}{(\hat{n}-1)!}  {}^{[n]}\delta^{\underline{ a_{1} }\dots \underline{ a_{\hat{n}} }\underline{ kk_{2} }\dots \underline{ k_{n} }}_{\underline{ b_{1} }\dots \underline{ b_{\hat{n}}bk_{2} }\dots \underline{ k_{n} }} }_{ \text{identita} }\nabla_{\underline{ k }}\mathfrak{a}^{\underline{ b_{1} }\dots \underline{ b_{\hat{n}} b\,}}\\
&={}^{[\hat{n}+1]}\delta^{\underline{ a_{1} }\dots \underline{ a_{\hat{n}}k\, }}_{\underline{ b_{1} }\dots \underline{ b_{\hat{n}}b \,}} \nabla_{\underline{ k }}\mathfrak{a}^{\underline{ b_{1} }\dots \underline{ b_{\hat{n}}b\, }}\\
&=\nabla_{\underline{ k }}\mathfrak{a}^{\underline{ a_{1} }\dots \underline{ a_{\hat{n}}k\, }}
\end{aligned}
\end{equation}
$$
### Lemma: Nezávislost Div
Následujeme konvenci, že $\cdot$ úží nejbližší indexy, a tedy $\nabla \cdot \mathfrak{a}$, nezávisí na beztorzní [CD](Kovariantn%C3%AD%20Derivace#D%20Kovariantní%20derivace%20(CD)).
Pro jakoukoliv beztorzní derivaci pak
$$
\begin{equation}
\begin{aligned}
(\nabla \cdot \mathfrak{a})_{x}^{a_{1}\dots a_{\hat{n}}}=(\partial \cdot \mathfrak{a})_{x}^{a_{1}\dots a_\hat{n}}=\partial_{k}\mathfrak{a}^{ka_{1}\dots a_{\hat{n}}},
\end{aligned}
\end{equation}
$$
neboli také
$$
\begin{equation}
\begin{aligned}
(\text{div}\mathfrak{a})_{x}^{a_{1}\dots a_{\hat{n}}}=\mathfrak{a}_{x}^{a_{1}\dots a_{\hat{n}}k},_{k}
\end{aligned}
\end{equation}
$$
**Speciálně:** $\mathrm{d}\cdot \mathfrak{a}=(-1)^{\hat{n}-1}~\text{div}\mathfrak{a}$
### D: Rotace
$$
\begin{equation}
\begin{aligned}
\tilde{\text{rot}}\alpha=\#\mathrm{d}\alpha
\end{aligned}
\end{equation}
$$
Ve smyslu hustoty, duál vnější derivace. 
$\mathrm{d}\alpha=\#\tilde{\text{rot}}\alpha$
$\text{div}\mathfrak{a}=\tilde{\text{rot}}\#\mathfrak{a}$
## Lieova derivace (TAS tenz.) hustot
[Lieova derivace](Tok%20a%20Lieova%20Derivace#D%20Lieova%20derivace) $\mathcal{L}_{a}$ jako tenzorovou derivaci rozšíříme na hustoty přirozenou podmínkou, že [derivace anihilují](Integrovateln%C3%A9%20hustoty#D%20Derivace%20hustot) [tenzor orientace](Integrovateln%C3%A9%20hustoty#D%20Tenzor%20orientace): $\mathcal{L}_{a}\tilde{\varepsilon}=0\iff \mathcal{L}_{a}\#\alpha=\#\mathcal{L}_{a}\alpha$.
Zároveň je [Lieova derivace](Tok%20a%20Lieova%20Derivace#D%20Lieova%20derivace) [určená na tenzorech](U%C5%BEite%C4%8Dn%C3%A9%20Vztahy%20Derivac%C3%AD#Vztah%20$\nabla$%20a%20$\mathcal{L}_{}$) pomocí [pseudoderivace](Tenzorová%20pole#D%20Pseudoderivace$mathbf(M)$), toto se přenáší stejné i na hustoty: $\mathcal{L}_{a}\mathfrak{m}=a\cdot \nabla \mathfrak{m}+{ \mathbf{L}_{a} }\mathfrak{m} =  a\cdot \nabla \mathfrak{m}-\nabla a\cdot \mathfrak{m}$ , kde už můžeme využít, jak pseudoderivace působí na hustotu přes [stopy pseudoderivace](Integrovatelné%20hustoty#^jzzs1h): $=a\cdot \nabla \mathfrak{m}-(-\nabla \cdot a)\mathfrak{m}$,
což vede na $\mathcal{L}_{a}\mathfrak{m}=\nabla \cdot(a\mathfrak{m})=\text{div}(a\mathfrak{m})$ (striktně vzato, $(a\mathfrak{m})\cdot \nabla$, ale má jen jeden index.)
### Lemma: Cartanovo lemma pro divergenci
Rozšíření [Cartanovy identity](Tok%20a%20Lieova%20Derivace#Věta%20Cartanova%20identita) na [integrovatelné hustoty](Integrovateln%C3%A9%20hustoty#D%20Integrovatelná%20hustota%20v%20bodě). Duální vzorec
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{u}\mathfrak{a}=(\text{div}\mathfrak{a})\wedge u+\text{div}(\mathfrak{a}\wedge u)
\end{aligned}
\end{equation}
$$
($\wedge$ provádíme zprava u tečných indexů)

**Dk:**
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{u}\mathfrak{a}&=\#\mathcal{L}_{u}\alpha\#\left(u\cdot \mathrm{d}\alpha+\mathrm{d}\left(u\cdot \alpha\right)\right)=\\
&=\left|\#(\mathfrak{a}\wedge u)=u\cdot(\#\mathfrak{a})\right|=\\
&= (\#\mathrm{d}\alpha)\wedge u+\#\mathrm{d}\#(\mathfrak{a}\wedge u)\\
&=(\text{div}\mathfrak{a})\wedge u+\text{div}(\mathfrak{a}\wedge u)
\end{aligned}
\end{equation}
$$

### D: Metrická divergence
Metrika $g$ , $\nabla$ bude [L-C](Levi-Civitova%20Derivace#Věta%20Levi-Civitova%20derivace) a $\text{g}^{1/2}$ je objemový element.
$$
\begin{equation}
\begin{aligned}
(\text{div}\omega)_{\underline{ a_{1} }\dots \underline{ a_{n} }}=\nabla^{\underline{ k }}\omega_{\underline{ a_{1} }\dots \underline{ a_{n}k \,}}
\end{aligned}
\end{equation}
$$
kde je už zvednutý index u [L-C](Levi-Civitova%20Derivace#Věta%20Levi-Civitova%20derivace) derivace.
$(\text{div}\omega)^{a_{1}\dots a_{n}}=\text{g}^{-1/2} \nabla_{k}(\text{g}^{1/2}\omega^{a_{1}\dots a_{n}})=\text{g}^{-1/2}(\text{div}\text{g}^{1/2}\omega)^{a_{1}\dots a_{n}}$
Ve výsledku to znamená
$$
\begin{equation}
\begin{aligned}
\text{g}^{1/2}\#~\text{div}\omega=\text{div}\text{g}^{1/2}\#\omega
\end{aligned}
\end{equation}
$$
na levo máme metrickou divergenci, na pravo ve smyslu hustotní divergence.

**Speciálně:**
$$
\begin{equation}
\begin{aligned}
*~\text{div}\omega=\mathrm{d}*\omega\longrightarrow \text{div}\omega=*^{-1}\mathrm{d}*\omega
\end{aligned}
\end{equation}
$$
### D: de Rhamův Laplaceův operátor
$$
\begin{equation}
\begin{aligned}
\Delta &= \mathrm{d}\mathrm{d}\cdot+\mathrm{d}\cdot \mathrm{d}\\
&=\nabla \wedge \nabla \cdot +\nabla \cdot \nabla \wedge
\end{aligned}
\end{equation}
$$
**Speciálně:**
$\Delta \omega=\mathrm{d}(\nabla \cdot \omega)+(-1)^{p}~\text{div}\mathrm{d}\omega=\mathrm{d}(\nabla \cdot \omega)\mp\text{rot}~\text{rot}~\omega$
Tady vidíme zobecnění známé struktury ze 3D.
### D: Weitzenböckova identita
$$
\begin{equation}
\begin{aligned}
-\Delta \omega_{a_{1}\dots a_{p}}=-\nabla^{2}\omega_{a_{1}\dots a_{p}}+p\mathrm{Ric}_{n[a_{1}}{\omega^{n}}_{a_{2}\dots a_{p}]}-\frac{p(p-1)}{2}R_{mn[a_{1}a_{2}} ~{\omega^{mn}}_{a_{3}\dots a_{p}}]
\end{aligned}
\end{equation}
$$
Od známého „Beltrami-Laplaceova” $\nabla^{2}$ se to navíc liší křivostí a křivostního potenciálu.