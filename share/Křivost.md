---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 19
cssclasses:
  - pdf-export
toc: true
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

# Křivost
### Lemma: Ultralokalita L-C Cartanova vzorce
Objekt je [ultralokální](Tenzorov%C3%A1%20pole#D%20Tenzorov%C3%A9%20pole%20jako%20univerz%C3%A1ln%C3%AD%20$%20mathcal%7BF%7DM$-line%C3%A1rn%C3%AD%20zobrazen%C3%AD) v $a,b,C$ pro $a,b$ vektorová pole, $C\in \mathcal{T}_{l}^{k}M$.
$$
\begin{equation}
\begin{aligned}
(\nabla_{a}\nabla_{b}-\nabla_{b}\nabla_{a}-\nabla_{[a;b]})C
\end{aligned}
\end{equation}
$$
__Dk:__ $\mathcal{F}M$-linearita 
Jedná se o jakési rozšíření (o [L-C](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace)) [Cartanova vzorce](Kovariantn%C3%AD%20Derivace#Lemma%20Ultralokalita%20Cartanova%20vzorce%20pro%20CD):  
$$
\begin{equation}
\begin{aligned}
(\nabla_{a}\nabla_{fb}-\nabla_{fb}\nabla_{a}-\nabla_{[a;fb]})C&=\\
&=(\nabla_{a}(f\nabla_{b})-f\nabla_{b}\nabla_{a}-\nabla_{[a;fb]})C\\
&=f(\nabla_{a}\nabla_{b}-\nabla_{b}\nabla_{a}-\nabla_{[a;b]})C\\
&\,\,+(\underbrace{ \cancel{ \nabla_{a}f }}_{ \nabla_{a\cdot \mathrm{d}f} })(\nabla_{b}C) -\cancel{ \nabla_{a\cdot \mathrm{d}f}\nabla_{b}C }
\end{aligned}
\end{equation}
$$
Ultralokalita v $C$, se dokáže později sama, neboť $(\dots)$ [objekt je pseudoderivace](#Lemma%20Oper%C3%A1tor%20k%C5%99ivost%20jako%20pseudoderivace).

### D: Riemannův tenzor
Pro $a,b,c$ vektorová pole a [L-C derivaci](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace) definujeme speciální antisymetrii derivací
$$
\begin{equation}
\begin{aligned}
R_{ab}c:=(\nabla_{a}\nabla_{b}-\nabla_{b}\nabla a-\nabla_{{[a;b]}})c.
\end{aligned}
\end{equation}
$$
_V indexech:_
$$
\begin{equation}
\begin{aligned}
(R_{a,b}c)^{\underline{ k }}=a^{\underline{ m }}b^{\underline{ n }}{{R_{\underline{ mn }}}^{\underline{ k }}}{}_{\underline{ l }}~c^{\underline{ l }}
\end{aligned}
\end{equation}
$$
_Také se používá napůl kontrahovaný:_ 
$$
\begin{equation}
\begin{aligned}
{R_{ab}}^{\underline{ k }}{}_{\underline{ l }}
\end{aligned}
\end{equation}
$$
jako operátor křivosti, ([za chvíli](#D%20Oper%C3%A1tor%20K%C5%99ivosti)).

_Kovariantním diferenciálem:_
$$
\begin{equation}
\begin{aligned}
{{R_{\underline{ mn }}}^{\underline{ k }}}{}_{\underline{ l }}~c^{\underline{ l }}=
\nabla_{\underline{ m }}\nabla_{\underline{ n }}c^{\underline{ k }}-\nabla_{\underline{ n }}\nabla_{\underline{ m }}c^{\underline{ k }}+{T_{\underline{ mn }}}^{\underline{ l }}~\nabla_{\underline{ l }}~c^{\underline{ k }}
\end{aligned}
\end{equation}
$$

### D: Operátor Křivosti
Definujeme pro vektorová pole $a,b$ a [L-C derivaci](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace), Křivost jako
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}&:=\nabla_{a}\nabla_{b}-\nabla_{b}\nabla_{a}-\nabla_{[a;b]}\\
&\,=a^{\underline{ m }}b^{\underline{ n }}~\mathbf{R}_{\underline{ mn }}\\
&\,= {R_{ab}}^{\underline{ k }}{}_{\underline{ l }}
\end{aligned}
\end{equation}
$$
_Kovariantním diferenciálem:_
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{\underline{ mn }}=\nabla_{\underline{ m }}\nabla_{\underline{ n }}-\nabla_{\underline{ n }}\nabla_{\underline{ m }}+{T_{\underline{ mn }}}^{\underline{ l }}\nabla_{\underline{ l }}
\end{aligned}
\end{equation}
$$
Poznámka.: Pro obecnou kovariantní derivaci, se může objevit torzní člen.

### Lemma: Operátor křivost jako pseudoderivace 
Operátor křivosti je [pseudoderivace](Tenzorov%C3%A1%20pole#D%20Pseudoderivace%20$%20mathbf%7BM%7D$). Tzn.: na vektorová pole $M,N$ a na skalární fci $f$:
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}(M\otimes N) &=\left(\mathbf{R}_{ab} M\right) N + M 
\left(\mathbf{R}_{ab} N\right)\,\,\,\, \text{(Leibnitz)}\\
\mathbf{R}_{ab}f&=0
\end{aligned}
\end{equation}
$$
Jakožto pseudoderivace je [určena akcí na tenzorech](Tenzorov%C3%A1%20pole#lemma%20$%20mathbf%7BM%7D$%20je%20d%C3%A1na%20akc%C3%AD%20na%20$%20mathcal%7BT%7D_%7B%7DM$), tedy pomocí napůl kontrahovaného [Riemannova tenzoru](#D%20Riemann%C5%AFv%20tenzor):
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{{ ab }} ~T^{\underline{ k }\dots}_{\underline{ l }\dots}={{{R}_{{ ab }}}^{\underline{ k }}}{}_{\underline{ n }}~T^{\underline{ n }\dots}_{\underline{ l }\dots}+\dots-{{{R}_{{ ab }}}^{\underline{ n }}}{}_{\underline{ l }}~T^{\underline{ k }\dots}_{\underline{ n }\dots}-\cdots
\end{aligned}
\end{equation}
$$
(Těmto rovnicím se také říká Ricciho identity)

__Dk:__
Jako pseudoderivace měří nekomutativitu derivací, což si ověříme:
- na vektorová pole působí jako derivace.
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}(MN) &= \left[(\nabla_a \nabla_b M) N + (\nabla_b M)(\nabla_a N) + (\nabla_a M)(\nabla_b N) + M(\nabla_a \nabla_b N)\right] \\ &\quad - \left[(\nabla_b \nabla_a M) N + (\nabla_a M)(\nabla_b N) + (\nabla_b M)(\nabla_a N) + M(\nabla_b \nabla_a N)\right] \\ &\quad - (\nabla_{[a,b]} M) N - M(\nabla_{[a,b]} N)\\
\\
&= \left[\nabla_a \nabla_b M - \nabla_b \nabla_a M - \nabla_{[a,b]} M \right] N \\ &\quad + M \left[ \nabla_a \nabla_b N - \nabla_b \nabla_a N - \nabla_{[a,b]} N \right]\\
\\
&= \left(\mathbf{R}_{ab} M\right) N + M \left(\mathbf{R}_{ab} N\right)
\end{aligned}
\end{equation}
$$
- funkce však anihiluje.
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}f=\nabla_{a}\nabla_{b}f-\nabla_{b}\nabla_{a}f-\nabla_{[a;b]}f=0
\end{aligned}
\end{equation}
$$
### D: Ricciho identity
Pro Operátor Křivosti $\mathbf{R}_{ab}$ a obecný tenzor $T$, platí rozpis do [Riemannova tenzoru křivosti](K%C5%99ivost#D%20Riemann%C5%AFv%20tenzor) 
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{{ ab }} ~T^{\underline{ k }\dots}_{\underline{ l }\dots}={{{R}_{{ ab }}}^{\underline{ k }}}{}_{\underline{ n }}~T^{\underline{ n }\dots}_{\underline{ l }\dots}+\dots-{{{R}_{{ ab }}}^{\underline{ n }}}{}_{\underline{ l }}~T^{\underline{ k }\dots}_{\underline{ n }\dots}-\cdots.
\end{aligned}
\end{equation}
$$

### Věta: Riemann pro různé CD
Pro souřadnicovou $x^{\mu}\to \partial$ … $\text{Tor}[\partial]=0$ … $\text{Riemann}[\partial]=0$.
Pro frejmovou $e_{k}\to \eth$ … $\text{Tor}[ \eth ]\neq{0}$ … $\text{Riemann}[\partial]=0$.

__Dk:__ $\mathbf{R}_{ab}e_{k}=\eth_{a}\eth_{b}e_{k}-\eth_{b}\eth_{a}e_{k}-\eth_{[a;b]}e_{k}=0$
Je vhodný trik pro spočtení Riemannova tenzoru. Stačí přidat $\Gamma$.



### Tvrzení: Riemann obecně
$$
\begin{equation}
\begin{aligned}
{{R_{\underline{ ab }}}^{\underline{ k }}}{}_{\underline{ l }}={{\tilde{R}_{\underline{ ab }}}^{\underline{ k }}}{}_{\underline{ l }}+\tilde{\nabla}_{\underline{ a }}\Gamma_{\underline{ bl }}^{\underline{ k }}-\tilde{\nabla}_{\underline{ b }}\Gamma_{\underline{ al }}^{\underline{ k }}+\tilde{T}^{\underline{ m }}_{\underline{ ab }}~\Gamma^{\underline{ k }}_{\underline{ ml }}+\Gamma^{\underline{ k }}_{\underline{ am }}~\Gamma^{\underline{ m }}_{\underline{ bl }}-\Gamma^{\underline{ k }}_{\underline{ bm }}~\Gamma^{\underline{ m }}_{\underline{ al }}
\end{aligned}
\end{equation}
$$
Obsahuje: Kovariantní derivace $\Gamma$, Torzní člen $T$, a členy dvojitých $\Gamma$.
_Speciálně:_
-  Přechod od $\partial\to \nabla$ $\nabla=\partial+\Gamma$ 
$$
\begin{equation}
\begin{aligned}
{{R_{\underline{ ab }}}^{\underline{ k }}}{}_{\underline{ l }}=0 + \Gamma_{\underline{ bl },\underline{ a }}^{\underline{ k }}-\Gamma_{\underline{ al},\underline{ b } }^{\underline{ k }}+\Gamma^{\underline{ k }}_{\underline{ am }}~\Gamma^{\underline{ m }}_{\underline{ bl }}-\Gamma^{\underline{ k }}_{\underline{ bm }}~\Gamma^{\underline{ m }}_{\underline{ al }}
\end{aligned}
\end{equation}
$$
-  Přechod od $\eth\to \nabla$ $\nabla=\eth+\gamma$, tj. Frejmová ale s [1-formou konexe](Levi-Civitova%20Derivace#D%201-Forma%20konexe%20pro%20Levi-Civitu) ${{\gamma_{\underline{ a }}}^k}{}_{l}\equiv {{\omega_{\underline{ a }}}^k}{}_{l}$:
$$
\begin{equation}
\begin{aligned}
{{R_{\underline{ ab }}}^{{ k }}}{}_{{ l }}&={\eth_{\underline{ a }}}{{\omega_{\underline{ b }}}^{{ k }}}_{l}-\eth_{b}{{\omega_{\underline{ a }}}^{{ k }}}_{l}+\mathscr{t_{\underline{ ab }}}^{\underline{ n }}{{\omega_{\underline{ n }}}^{{ k }}}{}_{l}\\
&~~~~~~+{\omega_{\underline{ a }}}^{{ k }}{}_{m}~{\omega_{\underline{ b }}}^{{ m }}{}_{l}-{\omega_{\underline{ b }}}^{{ k }}{}_{m}~{\omega_{\underline{ a }}}^{{ m }}{}_{l}\\ \\
\equiv {{\Omega_{\underline{ ab }}}^{k}}_{l}&= \mathrm{d}_{\underline{ a }}~{{\omega_{\underline{ b }}}^{k}}{}_{l}-{{\omega_{\underline{ b }}}^{k}}{}_{m}\wedge{{\omega_{\underline{ a }}}^{m}}{}_{l}
\end{aligned}
\end{equation}
$$
Tomu se také říká II. Cartanovy rovnice struktury.

# Rozštěpení Riemannova tenzoru s L-C derivací
Obecně lze psát součin symetrických tenzorů $\mathbf{T}_{(2)}^{0}M$v symetrizovaném tvaru:
##### D: Symetrizace tenzoru
$$
\begin{equation}
\begin{aligned}
&(A \wedge \wedge B)_{\underline{ abcd }}=4A_{[a|[c}B_{d]b]}\\
&=A_{ac}B_{db}+A_{bd}B_{ca}-A_{ad}B_{cb}-A_{bc}B_{da}
\end{aligned}
\end{equation}
$$
###### Def: Tr ze symetrizace
$$
\begin{equation}
\begin{aligned}
\text{Tr}[A\wedge \wedge B]=A\text{Tr}B+B\text{Tr}A -A\cdot B-B\cdot A
\end{aligned}
\end{equation}
$$
Speciálně $B=g$: $Tr(A\wedge \wedge g)=(d-2)A+g~\text{Tr}A$
Speciálněji $A=g$: $Tr(g\wedge \wedge g)=2(d-1)g$

### D: Bezestopá část Ricci
Vytáhneme si z Ricciho tenzoru bezestopou část $S$
$$
\begin{equation}
\begin{aligned}
\mathrm{Ric}=S+ \frac{1}{d}\mathscr{R}g,
\end{aligned}
\end{equation}
$$
kde koeficient $\frac{1}{d}$ vychází z identického vztahu $\mathscr{R}$ po aplikaci $\text{Tr}$.

### Věta: Rozštěpení Riemannova tenzoru
$$
\begin{equation}
\begin{aligned}
R = C + \frac{1}{d-2} \mathrm{Ric} \wedge \wedge g - \frac{1}{2(d-1)(d-2)}\mathscr{R}g\wedge \wedge g,
\end{aligned}
\end{equation}
$$
kde $C$ je analogická bezestopá část Riemannova tenzoru tzv. Weylův tenzor. Inverzním vztahem ho lze odtud vyjádřit.
Dk:
Začněme s extrakcí Bezestopé části $R$, a koeficientní úměrností
$$
\begin{equation}
\begin{aligned}
R = C +\alpha S\wedge \wedge g +\beta \mathscr{R}~g\wedge \wedge g
\end{aligned}
\end{equation}
$$
Postupnými aplikacemi $\text{Tr}$ se dozvíme povahu $\alpha, \beta$. Nakonec nahradíme $S$, Riccim. Zbytek Ricciho se schová ve členu $\beta$.


### D: Weylův tenzor
$$
\begin{equation}
\begin{aligned}
C_{\underline{ abcd }}=R_{\underline{ abcd }}- \frac{4}{d-2} \mathrm{Ric}_{[\underline{ a }|[\underline{ c }}g_{\underline{ d }]|\underline{ b }]}+ \frac{2\mathscr{R}}{(d-1)(d-2)}g_{[\underline{ a }|[\underline{ c }}g_{\underline{ d }]|\underline{ b }]}
\end{aligned}
\end{equation}
$$
(Pro konformně ploché prostoročasy $\implies$ $C=0$. Speciálně ve 3 dimenzích je identicky nulový, ale to neimplikuje, že jsou si všechny prostoročasy konformně ekvivalentní, je třeba přidat podmínku, kterou [později uvidíme](#Def%20Cotton%C5%AFv%20tenzor).)
## Další typy rozštěpení Riemannova tenzoru

### D: Schoutenův
$$
\begin{equation}
\begin{aligned}
\mathscr{Sch}=\frac{\mathscr{R}}{d-2}\left( \mathrm{Ric}_{}- \frac{1}{2(d-1)}\mathscr{R}g \right)
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\text{Tr}[\mathscr{Sch}]= \frac{\mathscr{R}}{2(d-1)}
\end{aligned}
\end{equation}
$$

### D: Einsteinův tenzor
$$
\begin{equation}
\begin{aligned}
\text{Ein}=\mathrm{Ric}_{}-\frac{1}{2}\mathscr{R}g
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
\text{Tr}[\text{Ein}]=- \frac{d-2}{2} \mathscr{R}g
\end{aligned}
\end{equation}
$$
### D: Cottonův tenzor
-  Antisymetrizace kovariantní derivace Schoutenova  (už derivace Riemanna) 
$$
\begin{equation}
\begin{aligned}
\mathscr{Cot}_{\underline{ amn }}=\nabla_{\underline{ m }}\mathscr{Sch}_{\underline{ na }}-\nabla_{\underline{ n }}\mathscr{Sch}_{\underline{ ma }}
\end{aligned}
\end{equation}
$$
- Z konstrukce je antisymetrický ve dvou zadních indexech, ale [Schoutenův tenzor](#D%20Schouten%C5%AFv) byl symetrický tenzor. Z čehož můžeme dedukovat:
$$
\begin{equation}
\begin{aligned}
\mathscr{Cot}_{[\underline{ amn }]}=0
\end{aligned}
\end{equation}
$$
- Divergence [Weylova tenzoru](#D%20Weyl%C5%AFv%20tenzor) je zbytek po bezestopé části [Riemanna](#D%20Riemann%C5%AFv%20tenzor). Ve třech dimenzích [Cottonův tenzor](#D%20Cotton%C5%AFv%20tenzor) nemusí být vůbec nulový, ale pro konformně ploché už musí být i on.
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ k }}{{C}_{\underline{ ab}}}^{\underline{ k }}{}_{\underline{ c }}=(d-3) \mathscr{Cot}_{\underline{ abc }}
\end{aligned}
\end{equation}
$$
- I v těch netriviálních indexech je stopa nulová.
$$
\begin{equation}
\begin{aligned}
{\mathscr{Cot}^{\underline{ n }}}_{\underline{ na }}=0
\end{aligned}
\end{equation}
$$
- v $d=3$ lze udělat duál a pracovat s tenzorem, který má dva indexy. 
$$
\begin{equation}
\begin{aligned}
^{*}\mathscr{Cot}^{\underline{ b }}_{\underline{ a }}= \frac{1}{2} \varepsilon^{\underline{ bkl }}\mathscr{Cot}_{\underline{ akl }}
\end{aligned}
\end{equation}
$$
Z Riemannova tenzoru má celou informaci o křivosti, ale lze extrahovat z něj další speciální informaci.
# Věta: Symetrie Riemannova tenzoru
Pro dimenze $d>2$ má Riemann počty komponent omezené symetriemi
$$
\begin{equation}
\begin{aligned}
{{R_{[ab]}}^{}}{}_{[cd]} \to \left(\frac{d(d-1)}{2}\right)^{2} \text{volností}
\end{aligned}
\end{equation}
$$
Další symetrie vychází z [Bianchiho identit](K%C5%99ivost#V%C4%9Bta%20Bianchiho%20identity), kterých je $d$ a to jsou zbylé nezávislé restrikce
$$
\begin{equation}
\begin{aligned}
{{R_{[abc]}}^{}}{}_{d}=0 \to \frac{d(d-1)(d-2)}{3}\cdot d \text{ vazeb}
\end{aligned}
\end{equation}
$$
což ve výsledku dává: $\frac{d^{2}(d^{2}-1)}{12}$.
### D: II. Cartanovy rovnice struktury
Určují Operátor křivosti, z 1-formy konexe, ([I. Cartanovy rovnice struktury](Levi-Civitova%20Derivace#D%20I.%20Cartanovy%20rovnice%20struktury))
$$
\begin{equation}
\begin{aligned}
{{\Omega_{\underline{ ab }}}^{k}}_{l}&= \mathrm{d}_{\underline{ a }}~{{\omega_{\underline{ b }}}^{k}}{}_{l}-{{\omega_{\underline{ b }}}^{k}}{}_{m}\wedge{{\omega_{\underline{ a }}}^{m}}{}_{l}
\end{aligned}
\end{equation}
$$
Též zvané 2-formy křivosti
### D: Cartanovy rovnice struktury
[I. Cartanovy rovnice struktury](Levi-Civitova%20Derivace#D%20I.%20Cartanovy%20rovnice%20struktury) určují 1-formu konexe:
$$
\begin{equation}
\begin{aligned}
\mathrm{d}e^{m}+{\omega^{m}}_{n}\wedge e^{n}=0
\end{aligned}
\end{equation}
$$

[II. Cartanovy rovnice struktury](#D%20II.%20Cartanovy%20rovnice%20struktury) určují [Operátor křivosti](#D%20Oper%C3%A1tor%20K%C5%99ivosti) z 1-formy konexe. 2-formy konexe
$$
\begin{equation}
\begin{aligned}
{{\Omega_{\underline{ ab }}}^{k}}_{l}&= \mathrm{d}_{\underline{ a }}~{{\omega_{\underline{ b }}}^{k}}{}_{l}-{{\omega_{\underline{ b }}}^{k}}{}_{m}\wedge{{\omega_{\underline{ a }}}^{m}}{}_{l}
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
{\Omega^{k}}_{l}=\mathrm{d}{\omega^{k}}_{l}+{\omega^{k}}_{n}\wedge {\omega^{n}}_{l}
\end{aligned}
\end{equation}
$$

### Stopy Riemannova tenzoru
V principu můžeme udělat dvě stopy. (1,2 s 3) nebo (3 s 4)
Kontrakci Riemannova tenzoru v jednom z prvních dvou indexech s třetím. Nazýváme Ricciho tenzor:
$$
\begin{equation}
\begin{aligned}
\mathrm{Ric}_{\underline{ ab }}=\mathrm{C}^{\underline{ d }}_{\underline{ c }}{{R_{\underline{ ad }}}^{\underline{ c }}}{}_{\underline{ b }}
\end{aligned}
\end{equation}
$$
Nestandardní stopa Riemanna:
$$
\begin{equation}
\begin{aligned}
\mathrm{Tr}[R]_{\underline{ ab }}={{R_{\underline{ ab }}}^{\underline{ c }}}{}_{\underline{ c }}
\end{aligned}
\end{equation}
$$
(V případě L-C derivace je identicky nula.)
### Lemma: Význam $\mathrm{Tr}[R_{ab}]$
V aplikaci na totálně antisymetrickou formu, tuto formu vnímá jako skalár. [Podobně jako u akce pseudoderivace na tot. antisymetrické formy](Metrick%C3%A1%20Struktura#Tot%C3%A1ln%C4%9B%20antisymetrick%C3%A9%20tenzory#Akce%20pseudoderivace), kde byla konstanta úměrnosti stopa tenzoru:
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}\omega_{a_{1}\dots a_{d}}=-\mathrm{Tr}[\mathbf{R}_{ab}]=\omega_{a_{1}\dots a_{d}}
\end{aligned}
\end{equation}
$$
Jelikož je Riemannova křivost komutace derivací, tak nám tato rovnice říká, jak na těchto „skalárech” derivace nekomutuji.

### Veta: Integrabilita $\nabla$ na $\mathcal{A}^{d}M$
$$
\begin{equation}
\begin{aligned}
\mathrm{Tr}\mathbf{R}=0 \iff \exists \omega \in \mathcal{A}^{d}M,\qquad\nabla \omega=0
\end{aligned}
\end{equation}
$$
Derivace zachovávající pojem objemu.

__Dk:__
$\impliedby$: je triviální.
$\implies$: $\mathrm{Tr}\mathbf{R}=0$ a také 
$$
\begin{equation}
\begin{aligned}
\nabla_{k}\tilde{\omega}_{a_{1}\dots a_{d}}=\lambda_{k}\tilde{\omega}_{a_{1}\dots a_{d}},
\end{aligned}
\end{equation}
$$
kde $\lambda$ spočteme z 
$$
\begin{equation}
\begin{aligned}
\lambda_{k}=\frac{1}{d!}(\nabla_{k}\tilde{\omega}_{a_{1}\dots a_{d}}){\tilde{\omega}^{-1}}^{a_{1}\dots a_{d}}.
\end{aligned}
\end{equation}
$$
Pak 
$$
\begin{equation}
\begin{aligned}
\mathbf{R}_{ab}\tilde{\omega}_{a_{1}\dots a_{d}}&=\nabla_{a}\nabla_{b}\tilde{\omega}_{a_{1}\dots a_{d}}-\nabla_{b}\nabla_{a}\tilde{\omega}_{a_{1}\dots a_{d}}+T^{k}_{ab}\nabla_{k}\tilde{\omega}_{a_{1}\dots a_{d}}=\\
&=\nabla_a\left(\lambda_b \tilde{\omega}_{a_1 \ldots a_d}\right)-\nabla_b\left(\lambda_a \tilde{\omega}_{a_1 \ldots a_d}\right)-T_{a b}^k \lambda_k \tilde{\omega}_{a_1 \ldots a_d} \\ & =\left[\left(\nabla_a \lambda_b-\nabla_b \lambda_a+T_{a b}^k \lambda_k\right)+\cancel{ \lambda_b \lambda_a-\lambda_a \lambda_b }\right] \tilde{\omega}_{a_1 \ldots a_d} \\ & =\mathrm{d}_a \lambda_b \tilde{\omega}_{a_1 \ldots a_d}=-\operatorname{Tr}\left[R_{a b}\right] \tilde{\omega}_{a_1 \ldots a_d} \\ & 0 \stackrel{!}{=} \operatorname{Tr}\left[R_{a b}\right]=-\mathrm{d}_a \lambda_b \Rightarrow \lambda=-\mathrm{d} f \\ & \omega:=e^{-f} \tilde{\omega} \\ & \begin{array}{l}\nabla \omega=e^{-f} \nabla \tilde{\omega}-e^{-f} \mathrm{d} f =e^{-f} \tilde{\omega}(\lambda-\mathrm{d} f)=0\end{array}.
\end{aligned}
\end{equation}
$$
Lokálně tedy $\mathrm{Tr}\mathbf{R}_{ab}$ je nula páč lambda má potenciál, který využijeme k přeškálování na nové $\omega$.
### Věta: Bianchiho identity
#### I. Bianchiho Identity
$$
\begin{equation}
\begin{aligned}
{{R_{[\underline{ ab }}}^{\underline{ n }}}{}_{\underline{ c }]}=\nabla_{[\underline{ a }}T^{\underline{ n }}_{\underline{ bc }}-T^{\underline{ k }}_{[\underline{ ab }}T^{\underline{ n }}_{\underline{ c }]\underline{ k }}
\end{aligned}
\end{equation}
$$
#### II. Bianchiho Identity
$$
\begin{equation}
\begin{aligned}
\nabla_{[\underline{ a }}{{R_{\underline{ bc }]}}^{\underline{ k }}}{}_{\underline{ l }}=T^{\underline{ n }}_{[\underline{ ab }}{{R_{\underline{ c}]\underline{ n }}}^{\underline{ k }}}{}_{\underline{ l }}
\end{aligned}
\end{equation}
$$
Speciálně, bez torze
I. ${{R_{[\underline{ ab }}}^{\underline{ n }}}{}_{\underline{ c }]}=0$
II. $\nabla_{[\underline{ a }}{{R_{\underline{ bc }]}}^{\underline{ k }}}{}_{\underline{ l }}=0$

__Dk:__
Pro beztorzní
I.
$$
\begin{equation}
\begin{aligned}
{{R_{ab}}^{n}}{}_{c}\omega_{n}=-\mathbf{R}_{ab}\omega_{c}=-\nabla_{[a}\nabla_{b}\omega_{c]}+\nabla_{[b}\nabla_{a}\omega_{c]}=-\nabla_{[a}(\nabla_{b}\omega_{c]}-\nabla_{c}\omega_{b]})=-\nabla_{[a}\mathrm{d}_{b}\omega_{c]}=-\frac{1}{3}\mathrm{d}_{a}\mathrm{d}_{b}\omega_{c}
\end{aligned}
\end{equation}
$$

II.
$$
\begin{equation}
\begin{aligned}
(\nabla_{a}{{R_{bc}}^{k}}{}_{l})~a^{l}&=\nabla_{a}(\underbrace{ {{R_{bc}}^{k}}{}_{l}~a^{l})}_{ \mathbf{R_{bc}}~a^{k} }\underbrace{ -{{R_{bc}}^{k}}{}_{l}~\nabla_{a}a^{l}+{{R_{bc}}^{l}{_{a}}}\nabla_{l}{a}^{k} }_{ -\mathbf{R}_{bc}\nabla_{a}~a^{k} }\overbrace{ -{{R_{bc}}^{l}}{_{a}}\nabla_{l}{a}^{k} }^{ \text{I. Bianchi = 0} }\\
&=\nabla_{a}\nabla_{b}\nabla _{c}a^{k} -\nabla_{a}\nabla_{c}\nabla_{b}a^{k}-\nabla_{b}\nabla_{c}\nabla_{a}a^{k}+\nabla_{c}\nabla_{b}\nabla_{a}a^{k}\\
[abc]\\
&=0
\end{aligned}
\end{equation}
$$
### Úžené Bianchiho identity
#### Zúžené B1:
$$
\begin{equation}
\begin{aligned}
2\mathrm{Ric}_{[ab]}+\mathrm{Tr}[\mathbf{R}_{ab}]=\mathrm{d}_{n}T^{n}_{ab}+\nabla_{n}T^{n}_{ab}
\end{aligned}
\end{equation}
$$
#### Zúžené B2 a):
$$
\begin{equation}
\begin{aligned}
\mathrm{d}_{a}\mathrm{Tr}[\mathbf{R}_{bc}]=0
\end{aligned}
\end{equation}
$$
#### Zúžené B2 b):
$$
\begin{equation}
\begin{aligned}
\nabla_{a}\mathrm{Ric}_{bc}-\nabla_{b}\mathrm{Ric}_{ac}+T^{n}_{ab}\mathbf{R}_{nc}=\nabla _{m}{{R_{ab}}^{m}}{}_{c}+T^{n}_{am}{{R_{bn}}^{m}}{}_{c}-T^{n}_{bm}{{R_{an}}^{m}}{}_{c}
\end{aligned}
\end{equation}
$$

Speciálně bez torze
$$
\begin{equation}
\begin{aligned}
2\mathrm{Ric}_{[ab]}=-\mathrm{Tr}[\mathbf{R}_{ab}]\\
\mathrm{d}_{a}\mathrm{Tr}R_{ab}=0\qquad&\qquad\qquad\nabla_{[a}\mathrm{Tr}[\mathbf{R}_{bc]}]=0\\
2\nabla_{[a}\mathrm{Ric}_{b]c}=\nabla_{m}{{R_{ab}}^{m}}{}_{c}
\end{aligned}
\end{equation}
$$


