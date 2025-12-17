---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 16
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

# Geodetiky
### D: Geodetika
Definujeme jako křivku, která se po přenesení podle sebe sama nemění.
$$
\begin{equation}
\begin{aligned}
u=\frac{\mathrm{D}z}{\mathrm{d}\tau},\,\text{pak}\,\frac{\nabla}{\mathrm{d}\tau}u^{\underline{ a }}=0  
\end{aligned}
\end{equation}
$$
_komentář:_ Tečný směr ke křivce $z(\tau)$ se při transportu vektoru ($\nabla u^{\underline{ a }}$) o $\mathrm{d}\tau$ nemění.

### Věta: Rovnice geodetiky v souřadnicích
- využití souřadnic $x^{k}\to \partial$. Pak posun ve směru křivky $u^{\underline{ m }}$ pomocí [diferenciálu](Kovariantní%20Derivace#D:%20Kovariantní%20diferenciál) vektoru $u^{\underline{ n }}$.
$$
\begin{equation}
\begin{aligned}
u^{\underline{ m }}\nabla_{\underline{ m }}u^{\underline{ n }}=u^{\underline{ m }}(\partial_{\underline{ m }}u^{\underline{ n }}+\Gamma^{\underline{ n }}_{\underline{ mk }}u^{\underline{ k }})\overset{\text{def}}=0.
\end{aligned}
\end{equation}
$$
Vyjádřeno v souřadnicích $x^{k}(\tau)\equiv x^{k}(z(\tau))$, takže $u^{k}=\frac{ \partial x^{k} }{ \partial \tau }$:
$$
\begin{equation}
\begin{aligned}
\frac{ \partial  }{ \partial \tau }\frac{ \partial u^{n} }{ \partial \tau }  +\Gamma^{n}_{mk}\frac{ \partial x^{m} }{ \partial \tau } \frac{ \partial x^{k} }{ \partial \tau }\overset{\text{def}}=0 .
\end{aligned}
\end{equation}
$$

### Věta: Ekvivalence geodetik pro (CD)
- Jak se liší [geodetiky](#D%20Geodetika) pro různé ([CD](Kovariantní%20Derivace#D%20Kovariantní%20derivace%20(CD))), tj. za jakých podmínek jsou [geodetiky](#D%20Geodetika) totožné?
Začněme z $\frac{ \tilde{\nabla}  }{ \mathrm{d}\tau }u=0\iff \frac{ {\nabla}  }{ \mathrm{d}\tau }u=0$ a aplikujme $\tilde{\nabla}+\Gamma=\nabla$. Dostáváme
$$
\begin{equation}
\begin{aligned}
&0\overset{\text{def}}=\cancelto{ 0 }{ \frac{ \tilde{\nabla} }{ \partial \tau } u^{\underline{ a }} }+\Gamma^{\underline{ a }}_{\underline{ mn }} u^{\underline{ m }}u^{\underline{ n }}\\
\iff&\Gamma^{\underline{ a }}_{\underline{ mn }} u^{\underline{ m }}u^{\underline{ n }}=0\,\,\iff\,\,\Gamma^{\underline{ a }}_{(\underline{ mn })}=0
\end{aligned}
\end{equation}
$$
čili pokud je symetrická část nulová.

### D: Afinní parametrizace
- Parametrizace [geodetiky](#D%20Geodetika) nemůže být libovolná. Pro $\eta=\eta(\tau)$ a tedy tečný vektor $\frac{ \mathrm{D}z }{ \mathrm{d}\tau }=\frac{ \mathrm{D}z }{ \mathrm{d}\eta }\frac{ \mathrm{d}\eta }{ \mathrm{d} \tau }$.
$\implies$
$$
\begin{equation}
\begin{aligned}
0\overset{\text{def}}=\frac{ \nabla }{ \mathrm{d}\tau }u^{\underline{ a }}
:= \frac{ \nabla }{ \mathrm{d}\tau }\frac{ \mathrm{D} z}{ \mathrm{d}\tau }
= \frac{ \mathrm{d}\eta }{ \mathrm{d} \tau }\frac{ \nabla }{ \mathrm{d}\eta }\left(\frac{ \mathrm{D} z}{ \mathrm{d}\eta }\frac{ \mathrm{d}\eta }{ \mathrm{d} \tau }\right)
=\frac{ \mathrm{d}^{2}\eta }{ \mathrm{d} \tau^{2} }\frac{ \mathrm{D} z}{ \mathrm{d}\eta }+\left(\frac{ \mathrm{d} \eta }{ \mathrm{d} \tau }\right)^{2}\frac{ \nabla }{ \mathrm{d}\eta }  \frac{ \mathrm{D}z }{ \mathrm{d}\eta },
\end{aligned}
\end{equation}
$$
což udává podmínku
$$
\begin{equation}
\begin{aligned}
\frac{ \mathrm{d}^{2}\eta }{ \mathrm{d} \tau^{2} }\underbrace{ \frac{ \mathrm{D} z}{ \mathrm{d}\eta } }_{ := \frac{\mathrm{d}x^{k}}{\mathrm{d}\eta} }+\left(\frac{ \mathrm{d} \eta }{ \mathrm{d} \tau }\right)^{2}\frac{ \nabla }{ \mathrm{d}\eta }  \frac{ \mathrm{D}z }{ \mathrm{d}\eta }&\overset!=0 
\\
\frac{ \mathrm{d}^{2}\eta }{ \mathrm{d} \tau^{2} }\frac{\mathrm{d}x^{k}}{\mathrm{d}\eta}+\left(\frac{ \mathrm{d} \eta }{ \mathrm{d} \tau }\right)^{2} \frac{ \mathrm{d}^{2} x^{k} }{ \mathrm{d}\eta^{2} } &\overset!=0
\\
\\
\iff \frac{ \mathrm{d}^{2} x^{k} }{ \mathrm{d}\eta^{2} }=-\frac{\mathrm{d}x^{k}}{\mathrm{d}\tau}\frac{\frac{ \mathrm{d}^{2}\eta }{ \mathrm{d} \tau^{2} }}{\left( \frac{\mathrm{d}\eta}{\mathrm{d}\tau} \right)^{2}}
&=\underbrace{ -\left( \frac{\mathrm{d}\tau}{\mathrm{d}\eta} \right)^{2}\frac{ \mathrm{d}^{2}\eta }{ \mathrm{d} \tau^{2} } }_{ \text{koef. úměrnosti} } \frac{\mathrm{d}x^{k}}{\mathrm{d}\tau}
\end{aligned}
\end{equation}
$$
je nově parametrizovaná [rovnice geodetiky](#D%20Geodetika) s koeficientem úměrnosti $\lambda(\eta)$, práva strana bude nulová pro _afinní transformaci_ $\eta(\tau)$ , tj. lineární transformaci $\eta= A\tau+B$.

### D: Geodetická souvislost
(jako konvexita v $\mathbb{R}^{n}$) Body v množině jsou dosažitelné geodetikou, která leží v množině.
### D: Normální okolí
Každý bod v okolí je jednoznačně dosažitelný.

### D: Geodetické zobrazení
$\mathscr{geod}_{\!\mathscr{x}}a=z(1)$, $\frac{\mathrm{D}z(0)}{\mathrm{d}\tau}=:a$, $\mathscr{x}:=z(0)$. 

Takové zobrazení nemusí být jednoznačné na celé varietě, (kaustiky). Bez protínání křivky $\exists$ [normální okolí](#D%20Normální%20okolí), kde je $\mathscr{geod}_{\!\mathscr{x}}$ 1-1značné a [ geodeticky souvislé](#D%20Geodetická%20souvislost).
Na takovém okolí volíme _normální souřadnice_ jakožto tečné směry [geodetik](#D%20Geodetika) v bodě $\mathscr{{}^{0}\!x}$. Volíme bázi v $\mathbf{T}_{\mathscr{{}^{0}\!x}}M$: $e_{k}$. Bodům $\mathscr{x}$ z okolí ${^{0}\!\mathscr{x}}$ přiřadím souřadnice $a$.
$$
\begin{equation}
\begin{aligned}
\mathscr{x}\mapsto a^{k}\equiv \bar{x}^{k}\,\,\, \text{kde}\,\,\,a=a^{k}e_{k}
\end{aligned}
\end{equation}
$$
