---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 17
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

# Levi-Civitova derivace
Přidáním $2$ dodatečných požadavků k obecné [CD](Kovariantní%20Derivace#D%20Kovariantní%20derivace%20(CD)).
1) [Paralelní přenos](Kovariantní%20Derivace#D%20Paralelní%20přenos) zachová úhly a délky:
$$
\begin{equation}
\begin{aligned}
0&\overset!= \frac{\nabla}{\mathrm{d}\tau}(a^{\underline{ a }}g_{\underline{ ab }}b^{\underline{ b }})=\frac{\nabla a^{\underline{ a }}}{\mathrm{d}\tau}g_{\underline{ \underline{ ab } }}b^{\underline{ b }} +a^{\underline{ a }}g_{\underline{ \underline{ ab } }}\frac{\nabla b^{\underline{ b }}}{\mathrm{d}\tau}+
a^{\underline{ a }}\frac{\nabla }{\mathrm{d}\tau}g_{\underline{ \underline{ ab } }}~b^{\underline{ b }} \\
&\iff\\
\frac{\nabla }{\mathrm{d}\tau}g_{\underline{ { ab } }}&=0,\qquad \forall z(\tau),
\end{aligned}
\end{equation}
$$
tzn.:
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ k }}g_{\underline{  ab  }}=0
\end{aligned}
\end{equation}
$$
2) Beztorzní derivace

### Věta: Levi-Civitova derivace
Podmínky [kovariantní derivace](Kovariantní%20Derivace#D%20Kovariantní%20derivace%20(CD)), zachování úhlu a délky při [paralelním přenosu](Kovariantní%20Derivace#D%20Paralelní%20přenos) a nulovost [torze](Kovariantní%20Derivace#D%20Torze), určuje Levi-Civitovu derivaci jednoznačně.
Jedná se o souřadnicovou a metrickou (Beztorzní a metriku anihilující)
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ k }}g_{\underline{  ab  }}=0,\qquad\qquad \text{Tor}[\nabla]=0
\end{aligned}
\end{equation}
$$


__Dk:__
Představme si $\nabla=\tilde{\nabla}+\gamma$. Označme $\text{Tor}[\tilde{\nabla}]\equiv \mathbfscr{t}$, ${\mathbfscr{t}^{\underline{ c }}}_{\underline{ ab }}$. Můžeme snížit metrikou.
$$
\begin{equation}
\begin{aligned}
\gamma=\nabla-\tilde{\nabla}\implies 0-\mathbfscr{t}_{\underline{ mab }}=\gamma_{\underline{ amb }}-\gamma_{\underline{ bma }}
\end{aligned}
\end{equation}
$$
Dále by pak $0 \overset?= \tilde{\nabla}_{\underline{ m }}g_{\underline{ ab }}=\cancelto{ 0 }{ \nabla_{\underline{ m }}g_{\underline{ ab }} }-(-\gamma_{\underline{ mab }}-\gamma_{\underline{ mba }})$. Jak $\gamma$ vypadají? Lze zjistit ze sudoku:
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ a }} g_{\underline{ bc }}&=\gamma_{\underline{ abc }}+\gamma_{\underline{ acb }}, \\
\nabla_{\underline{ b }} g_{\underline{ ca }}&=\gamma_{\underline{ bca }}+\gamma_{\underline{ bac }}, \\
\nabla_{\underline{ c }} g_{\underline{ ab }}&=\gamma_{\underline{ cab }}+\gamma_{\underline{ cba }}.
\end{aligned}
\end{equation}
$$
Manipulací s řádky, můžeme dostat symetrickou a antisymetrickou část v krajních indexech:
$$
\begin{equation}
\begin{aligned}
\gamma_{(a|c|b)}=&~~\phantom{+}\frac{1}{2}(\tilde{\nabla}_{\underline{ a }}g_{\underline{ bc }}+\tilde{\nabla}_{\underline{ b }}g_{\underline{ ac }}-\tilde{\nabla}_{\underline{ c }}g_{\underline{ ab }})\\
&+\frac{1}{2}(\mathbfscr{t}_{\underline{ abc }}+\mathbfscr{t}_{\underline{ bac }})\\
\\
\gamma_{[a|c|b]}=&-\frac{1}{2}(\mathbfscr{t}_{\underline{ cab }})
\end{aligned}
\end{equation}
$$
Dohromady dostáváme tvar $\gamma$ jako
$$
\begin{equation}
\begin{aligned}
{{{\gamma}_{\underline{ a }}}^{\underline{ k }}}_{\underline{ b }}=
&~~\phantom{+}\frac{1}{2}(\tilde{\nabla}_{\underline{ a }}g_{\underline{ bc }}+\tilde{\nabla}_{\underline{ b }}g_{\underline{ ac }}-\tilde{\nabla}_{\underline{ c }}g_{\underline{ ab }})\\
&+\frac{1}{2}(\mathbfscr{t}_{\underline{ abc }}+\mathbfscr{t}_{\underline{ bac }}
-\mathbfscr{t}_{\underline{ cab }})
\end{aligned}
\end{equation}
$$
Pokud chceme, aby
### D: 1-Forma konexe pro Levi-Civitu
Zvolíme si $n$-ádu $e_{n}$, resp. $e^{n}$, ve které vyjádříme poslední dva indexy [konexe](Kovariantní%20Derivace#D%20Konexe) a tím zbyde $1$-forma, které nazýváme _1-formy konexe_ $\omega$. Pro [Levi-Civitovu derivaci](#Věta%20Levi-Civitova%20derivace): (která je frejmová)$\wedge\cdot$
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ a }}e^{m}_{\underline{ b }}=-{{\gamma_{\underline{ a }}}^{\underline{ k }}}_{\underline{ b }}~e_{\underline{ k }}^{m}=-{{\gamma_{\underline{ a }}}^{m}}_{n}~e^{n}_{\underline{ b }}=:({\omega^{m}}_{n})_{\underline{ a }}e^{n}_{\underline{ b }}
\end{aligned}
\end{equation}
$$
(a je beztorzní)
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ a }}\wedge e_{\underline{ b }}^{m}=\mathrm{d}_{\underline{ a }}e^{m}_{\underline{ b }}={{\omega_{\underline{ a }}}^{m}}_{n}\wedge {e^{n}}_{\underline{ b }}
\end{aligned}
\end{equation}
$$
Můžeme vztah obrátit k nalezení takových $\omega$.

### D: I. Cartanovy rovnice struktury
$$
\begin{equation}
\begin{aligned}
\mathrm{d}e^{m}+{\omega^{m}}_{n}\wedge e^{n}=0
\end{aligned}
\end{equation}
$$
pomocí takových 1-forem $\omega$ lze dále napočítat křivost.
Speciální volba $n$-ády $e_{m}$, kdy je $g_{kl}=\text{konst.}$, pak budou $\omega_{[mn]}=\omega_{mn}$ plně antisymetrické v těchto souřadnicích.





