---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 6
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

# Tenzory
konstruovat lze opět různými způsoby. Skrze tenzorový součin VP. Nebo jako univerzální lineární zobrazení.
#### D: Tenzor
Pro $V_1, V_{2},\dots ,V_{k}$ VP nad $\mathbb{K}$, konstruujeme tenzorovým součinem
$$
\begin{equation}
\begin{aligned}
V_{1}\otimes V_{2} \otimes \dots \otimes V_{k}
\end{aligned}
\end{equation}
$$
prostor, kterému rozumíme, jako volné násobení prvků z $V_{i}$ vymezené na podprostor daný pravidly:
$$
\begin{equation}
\begin{aligned}
\ [1,\dots,r{j},\dots ,k]&=r[1,\dots,j,\dots,k]\\
[1,\dots,j+l,\dots ,k]&=[1,\dots,j,\dots,k]+[1,\dots,l,\dots,k]\\
\end{aligned}
\end{equation}
$$


Tenzorový součin obecně nekomutuje, je třeba hlídat třeba pomocí abstraktních indexů $\underline{ a }$, pro další použití indexy nebudeme podtrhávat, pokud je nebude nutné odlišit. Příklad:
$$
\begin{equation}
\begin{aligned}
(u\otimes v\otimes \phi)^{abA}=u^{a}\otimes v^{b}\otimes \phi^{A}\in V\times V\times U
\end{aligned}
\end{equation}
$$


#### D: Tenzory typu (K,L) nad V
$$
\begin{equation}
\begin{aligned}
V^{K}_{L}:=\underbrace{ V\otimes \dots \otimes V }_{ K }\otimes \underbrace{ V^{*}\otimes \dots \otimes V^{*} }_{ L }
\end{aligned}
\end{equation}
$$
horními indexy označujeme objekty chápané jako vektory, dolními pak kovektory.

##### Operace na tenzorech
Lineární operace je dána akcí na součinových tenzorech.
- __Úžení__ $\mathbf{\alpha}\cdot \mathbf{a}:V\times V^{*}\to \mathbb{R}$
$$
\begin{equation}
\begin{aligned}
\text{C}^{j}_{i}:V^{K}_{L}\to V^{K-1}_{L-1}
\end{aligned}
\end{equation}
$$
akce:
$C^{j}_{i}a_1\dots a_j\dots a_k\alpha^{1}\dots\alpha^{i}\dots\alpha^{L}= (a_{j}\cdot\alpha^{i})a_1\dots \cancel{ a_j }\dots a_k\alpha^{1}\dots\cancel{ \alpha^{i} }\dots\alpha^{L}$

- __Stopa__ : $A^{k}_{k}=:\text{Tr}[A]$
- __Algebraické operátory__ :
$$
\begin{equation}
\begin{aligned}
A :V\to V\\
b^{k}&=A^{k}_{l}a^{l}\\
b&=A\cdot a
\end{aligned}
\end{equation}
$$
- __Identita__ : $\delta^{m}_{n}$
- __Permutace na__ $V^{K}$ :
$$
\begin{equation}
\begin{aligned}
P_{\sigma}:V^{K}\to V^{K}
\end{aligned}
\end{equation}
$$
akce: $P_{\sigma}(u_{1}\dots u_{k})=u_{\sigma_{1}\dots\sigma_{K}}$
je jedno jestli permutujeme číslování $u_{i}$ nebo abstraktní indexy.

- __Symetrizace__ :
$$
\begin{equation}
\begin{aligned}
\mathscr{S}T=\frac{1}{k!}\sum_{\sigma}P_{\sigma}T
\end{aligned}
\end{equation}
$$
akce: $(\mathscr{S}T)^{a_{1}\dots a_{k}}=T^{(a_{1}\dots a_{k})}$
- __Antisymetrizace__ :
$$
\begin{equation}
\begin{aligned}
\mathscr{A}T=\frac{1}{k!}\sum_{\sigma}\text{sign}[\sigma]P_{\sigma}T
\end{aligned}
\end{equation}
$$
akce: $(\mathscr{A}T)^{a_{1}\dots a_{k}}=T^{[a_{1}\dots a_{k}]}$

#### Prostor antisymetrických tenzorů $V^{[K]}\subset V^{K}$
$$
\begin{equation}
\begin{aligned}
A \in V^{[K]} \iff \mathscr{A}A = A
\end{aligned}
\end{equation}
$$
identitu zde definujeme jako:
$$
\begin{equation}
\begin{aligned}
&^{[K]}\delta\in V_{[K]}\otimes V^{[K]}\\
&^{[K]}\delta^{b_{1}\dots b_{K}}_{a_{1}\dots a_{K}}=\delta^{[b_{1}}_{a_{1}}\otimes \dots \otimes \delta^{b_{K}]}_{a_{K}}
\end{aligned}
\end{equation}
$$
Tato identita je projektorem na $V^{[K]}$. tj. obecný tenzor antisymetrizuje.

#### Komponenty (souřadnice) Tenzorů
Vyjádřením tenzoru v nějaké bázi $V\, |\,e_{k}$, resp. $V^{*}\,|\,e^{k}$, $k=1,\dots,d$. Obdržíme komponenty tenzoru v dané bázi. $V^{K}_{L}\,|\,e_{m_{1}}\dots e_{m_{K}}e^{n_{1}}\dots e^{n_{L}}$.
$$
\begin{equation}
\begin{aligned}
T = T^{a^{1}\dots a^{K}}_{b^{1}\dots b^{L}} e_{a^{1}}\cdots e_{a^{K}}e^{b^{1}}\cdots e^{b^{L}}
\end{aligned}
\end{equation}
$$
pokud bychom Tenzor vypsali pomocí abstraktních indexů (musíme rozlišit souřadnice a abstraktní indexy). Souřadnice získáme jako:
$$
\begin{equation}
\begin{aligned}
T^{m_{1}\dots m_{K}}_{n_{1}\dots n_{L}}
=T^{\underline{ c }_{1}\dots\underline{ c }_{K}}_{\underline{ \gamma }_{1}\dots\underline{ \gamma }_{L}}~e^{\underline{ \gamma }_{1}}_{n_{1}}\cdots e^{\underline{ \gamma }_{L}}_{n_{L}}~e_{\underline{ c }_{1}}^{m_{1}}\cdots e_{\underline{ c }_{K}}^{m_{K}}.
\end{aligned}
\end{equation}
$$

#### Transformace v komponentách (souřadnicích)
Dvě různé komponentové báze rozlišíme čárkováním indexů.
$$
\begin{equation}
\begin{aligned}
e_{k'}={M}^{l}_{k'}e_{l}
\end{aligned}
\end{equation}
$$
Transformace souřadnic tenzoru je pak z linearity analogická:
$$
\begin{equation}
\begin{aligned}
T^{m_{1}'\dots m_{K}'}_{n_{1}'\dots n_{L}'}
= T^{a_{1}\dots a_{K}}_{b_{1}\dots b_{L}} M^{m_{1}'}_{a_{1}}\cdots M^{m_{k}'}_{a_{k}}  ~M^{b_{1}}_{n_{1}'}\cdots M^{b_{L}}_{n_{L}'}
\end{aligned}
\end{equation}
$$
speciálně lze vidět, jak se vypadají identity v různých souřadnicových bázích a jako přechodové.
$$
\begin{equation}
\begin{aligned}
\delta^{l}_{m}e_{l}e^{m}=M^{l}_{k'}e_{l}e^{k'} = \delta^{l}_{k'}e_{l}e^{k'}
\end{aligned}
\end{equation}
$$
tedy $\delta^{l}_{k'}=\delta^{\underline{ c }  }_{\underline{ d } }e^{l}_{\underline{ c }}~e_{k'}^{\underline{ d }}=e^{l}\cdot e_{k'}$


### D: Tenzory jako lineární zobrazení
$$
\begin{equation}
\begin{aligned}
l: V^{K_{1}}_{L_{1}}\times V^{K_{2}}_{L_{2}}\times\cdots\to V^{K}_{L}
\end{aligned}
\end{equation}
$$
(Velkým písmenem označíme kombo index)
$$
\begin{equation}
\begin{aligned}
l^{\underline{ A }}(T_{1},T_{2},\dots)=L^{\underline{ A }}_{\underline{ A }_{1}\underline{ A }_{2}\dots} T_{1}^{\underline{ A }_{1}}T_{2}^{\underline{ A }_{2}}\cdots
\end{aligned}
\end{equation}
$$

každé lineární zobrazení lze zadat tenzorem, příklad:
$$
\begin{equation}
\begin{aligned}
l: V\times V^{1}_{2}\to V^{*}
\end{aligned}
\end{equation}
$$
$$
\begin{equation}
\begin{aligned}
l_{\underline{ a }}(u,A)=L^{\underline{ m }\underline{ n }}_{\underline{ a }\underline{ p }\underline{ q }}
~ u^{\underline{ p }}
~ A^{\underline{ q }}_{\underline{ m }\underline{ n }}
\end{aligned}
\end{equation}
$$
v souřadnicové bázi:
$$
\begin{equation}
\begin{aligned}
e^{\underline{ a }}_{a}
~ l_{\underline{ a }}(u,A)
&= u^{p} 
~ A^{q}_{mn}
~ e^{\underline{ a }}_{a}~l_{\underline{ a }}(e_{p},e_{q},e^{m},e^{n})\\
&= u^{p}
~ A^{q}_{mn}
~ L^{mn}_{apq}
\end{aligned}
\end{equation}
$$

Můžeme definovat _multilineární_ zobrazení
$$
\begin{equation}
\begin{aligned}
A: V\times\dots \times V\times V^{*}\times\dots \times V^{*}\to \mathbb{R}
\end{aligned}
\end{equation}
$$
jako
$$
\begin{equation}
\begin{aligned}
A(a_{1},\dots a_{K},\alpha^{1},\dots\alpha^{L})
:= A^{\underline{ n }_{1}\dots\underline{ n }_{L}}_{\underline{ m }_{1}\dots\underline{ m }_{K}}
~ a^{\underline{ m }_{1}}_{1}\dots a^{\underline{ m }_{K}}_{K}
~ \alpha^{1}_{\underline{ n }_{1}}\dots \alpha^{L}_{\underline{ n }_{L}}
\end{aligned}
\end{equation}
$$



