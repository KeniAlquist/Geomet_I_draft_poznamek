---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 13
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

# Metrická struktura

### D: Základní Metrika

Metrika $g$ je $(0,2)$-tenzorové pole:
$$
\begin{equation}
\begin{aligned}
 g\in\mathcal{T}^0_2M,
\end{aligned}
\end{equation}
$$
které je 
- bilineární, 
- symetrické a 
- nedegenerované. 

Typy:
- __Riemannova__: pozitivně definitní ($a^{\underline{ m }}a^{\underline{ n }}g_{\underline{ mn }}>0$ pro $a\neq0$),
- __pseudoriemannova (Lorentzovská)__: signatura $(p,m)$, typicky $(-,+,\dots,+)$.

__Pozn.:__ $g$ indukuje izomorfismy, zvyšování a snižování
$$
\begin{equation}
\begin{aligned}
\flat: a\mapsto a^{\flat}:=g_{\underline{ mn }}a^{\underline{ n }},\qquad
\sharp: \alpha\mapsto \alpha^{\sharp}:={g^{-1}}^{\underline{ mn }}\alpha_{\underline{ m }},\quad g^{\underline{ ik }}g_{\underline{ kj }}=\delta^\underline{ i }{}\underline{ _j }.
\end{aligned}
\end{equation}
$$

### D: Délka křivky a vzdálenost
Pro křivky, jejichž kauzální struktura se nemění $z:[\tau_{z},\tau_{k}]\to M$, $u:=\tfrac{\mathrm d z}{\mathrm d\tau}$:
$$
\begin{equation}
\begin{aligned}
\Delta s=\int_{(\tau_{z},\tau_{k})} |{u^{\underline{ m }}u^{\underline{ n }}g_{\underline{ mn }}}|^{1/2}\,\mathrm d\tau
=\int_{(\tau_{z},\tau_{k})} \left|\frac{\mathrm{D}z}{\mathrm{d}\tau}\right|\,\mathrm d\tau=\int_{(\tau_{z},\tau_{k})} \mathrm ds.
\end{aligned}
\end{equation}
$$


## Totálně antisymetrické tenzory
mají tolik indexů kolik je dimenze $d$
$V^{[d]}\qquad V_{[d]}$, $\text{dim}V^{[d]}=1\text{dim}V_{[d]}$
$V_{[d]}\qquad \epsilon =e^{1}\wedge\dots\wedge e^{d}=d!\mathcal{A}(e^{ 1 }\dots e^{ d })$
$V^{[d]}\qquad e =e_{1}\wedge\dots\wedge e_{d}$

Připomenutí: $\epsilon \bullet e =1$, $\frac{1}{d!}\epsilon_{a_{1},\dots,a_{d}}e^{a_{1},\dots,a_{d}}=1$.

##### V souřadnicích:
$$
\begin{equation}
\begin{aligned}
e=\frac{ \partial  }{ \partial x^{1} }\wedge\dots \wedge \frac{ \partial  }{ \partial x^{d} },\qquad\epsilon=\mathrm{d}x^{1}\wedge\dots \wedge \mathrm{d}x^{d}.
\end{aligned}
\end{equation}
$$

Pak se tenzory
$$
\begin{equation}
\begin{aligned}
\omega=\omega_{1\dots d}\epsilon,\qquad w=w^{1,\dots,d}e,
\end{aligned}
\end{equation}
$$
kontrahují přes binární operaci
$$
\begin{equation}
\begin{aligned}
\omega \bullet w=\omega_{1,\dots,d}w^{1,\dots,d}
\end{aligned}
\end{equation}
$$
###### Změna báze:
$\omega=\omega_{1,\dots,d}\epsilon=\omega_{1',\dots,d'}\epsilon'$, tzn.:
$\omega_{1',\dots,d'}=T_{1'}^{a_{1}}\dots T_{d'}^{a_{d}}\omega_{a_{1},\dots, a_{d}}$                       (formálně přes jakýkoliv možné indexy $a_{i}$)
      $\qquad=\sum_{\sigma}\text{sign}[\sigma]T^{\sigma_{1}}_{1'}\dots T^{\sigma_{d}}_{d'}\omega_{1,\dots,d}~$      (Antisymetrická kombinace transformačních matic)
      $\qquad=\text{det}[T^{l}_{k'}]\omega_{1,\dots,d}$                                 (determinant přechodové matice, dává smysl, že 1 dim objekt se škáluje podobně jako číslo, ale nejsou to čísla.)
$w^{1',\dots,d'}=(\text{det}[T^{k'}_{l}])w^{1,\dots,d}$

######  inverze: 
${}^{-1}:$ $V^{[d]}\longleftrightarrow V_{[d]}$, $\omega^{-1}\bullet\omega=1$, $w^{-1}\bullet w=1$ (nejedná se však o __dualitu__)
$(\omega^{-1})^{1,\dots,d}=\frac{1}{\omega_{1,\dots,d}}$.

##### Akce pseudoderivace:
$\mathbf{M}\omega_{a_{1}\dots a_{d}}=-{d}M^{k}_{[a_{1}}\omega_{|k|\dots a_{d}}\overset!=\alpha \omega_{a_{1}\dots a_{d}}$  $/\bullet \omega^{-1}$
Faktor, který je dán stopou pseudoderivace:
$\alpha=\frac{d}{d!}{\omega^{-1}}^{a_{1}\dots a_{d}}M^{k}_{a_{1}}\omega_{ka_{2}\dots a_{d}}=dM^{k}_{a_{1}}~{}^{[d]}\delta_{ka_{2}\dots a_{d}}^{a_{1}\dots a_{d}}=M_{a}^{k}\delta_{k}^{a}=M^{n}_{n}$
(det je reprezentace akce grupy, a Tr je reprezentace příslušné algebry.)
##### D: Orientace báze
Báze $e_{k}$, $e_{k'}$, mají stejnou orientaci, pakliže $\text{det}[T^{k}_{k'}]>0$. Můžeme tedy v jednom bodě tvořit dvě třídy bazí.
Lze hovořit i o Globální orientovatelnosti.
Pokud $\omega_{1,\dots,d}>0$ tak, pozitivně orientovaná. Tedy i Totálně antisymetrické se děli do tříd.

### D: Levi-Civitův tenzor
Positivně orientovaná
$$
\begin{equation}
\begin{aligned}
\varepsilon_{a_1\dots a_d}\in\Lambda^{d}M\\
\varepsilon_{a_{1}\dots a_{d}}{\varepsilon^{\sharp}}^{a_{1},\dots a_{d}}=\text{sign}[g]d!,\qquad \varepsilon \cdot\varepsilon^{\sharp}=\text{sign}[g]
\end{aligned}
\end{equation}
$$

### D: Hodgeův duál $*$
Zobrazení z forem stupně $p$ do $d-p$
$$
\begin{equation}
\begin{aligned}
*:\Lambda^{p}M\to\Lambda^{d-p}M
\end{aligned}
\end{equation}
$$

Pro $\omega\in\Lambda^pM$ definujeme $*\alpha\in\Lambda^{d-p}M$ jako
$$
\begin{equation}
\begin{aligned}
(*\omega)_{a_{p+1}\dots a_{d}}=\frac{1}{p!}{\omega^{\sharp}}^{a_{1}\dots a_{p}}\varepsilon_{a_{1}\dots a_{d}}.
\end{aligned}
\end{equation}
$$
V souř. s metrikou:
$$
\begin{equation}
\begin{aligned}
(*\alpha)_{i_{k+1}\dots i_n}=\tfrac{1}{k!}\,\varepsilon_{i_1\dots i_n}\,g^{i_1j_1}\cdots g^{i_kj_k}\,\alpha_{j_1\dots j_k}.
\end{aligned}
\end{equation}
$$
Hodge je skoro inverzní, až na znaménko !!
$$
\begin{equation}
\begin{aligned}
**\omega=(\text{sign}[g])(-1)^{p(d-p)}~\omega
\end{aligned}
\end{equation}
$$
Díky němu mi dovolí udělat něco jako skalární součin na antisymetrických formách
$$
\begin{equation}
\begin{aligned}
(*\omega)\bullet(*\sigma)=(\text{sign}[g])~\omega\bullet\sigma
\end{aligned}
\end{equation}
$$
symetrický a dává skalární faktor
$$
\begin{equation}
\begin{aligned}
\omega \wedge (*\sigma)=\sigma \wedge (*\omega)=\omega\bullet\sigma ~~\varepsilon=*(\omega\bullet\sigma)
\end{aligned}
\end{equation}
$$
Takto můžeme hovořit jako o duál skaláru ($\omega\bullet\sigma$).
