---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 15
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

# Užitečné vztahy derivací
## Vztah $\nabla$ a $\mathcal{L}_{}$
Připomenutí [CD](Kovariantn%C3%AD%20Derivace#D%20Kovariantn%C3%AD%20derivace%20(CD)) a [Lieovy derivace](Tok%20a%20Lieova%20Derivace#D%20Lieova%20derivace). Přechodem mezi derivacemi skrze [Lieovu závorku]((Ko)Vektorov%C3%A1%20Pole#D%20Lieova%20z%C3%A1vorka) a definici [torze](Kovariantn%C3%AD%20Derivace#D%20Torze):
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}b^{\underline{ n }}&=[a;b]^{\underline{ n }}= a^{\underline{ k }}\nabla_{\underline{ k }}b^{\underline{ n }}\underbrace{ -b^{\underline{ k }}\nabla_{\underline{ k }}a^{\underline{ n }} - T^{\underline{ n }}_{\underline{ mk }}a^{\underline{ m }}b^{\underline{ k }} }_{ \equiv \mathbf{L}^{\underline{ n }}_{a^{\underline{ k }}} }y
\\
\\
\mathcal{L}_{a}&=\nabla_{a}+\mathbf{L}_{a}.
\end{aligned}
\end{equation}
$$

__Speciálně:__ když $T=0$, pak 
- pro $1$-formu:
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}\omega_{\underline{ m }}&=\nabla_{a}\omega_{\underline{ m }}-\mathbf{L}^{\underline{ n }}_{a^{\underline{ m }}}\omega_{\underline{ n }}=\\
&=\nabla_{a}\omega_{\underline{ m }}+(\nabla_{\underline{ m }}a^{\underline{ n }})\omega_{\underline{ n }}
\end{aligned}
\end{equation}
$$
- pro $2$-formu (např. metrika):
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}g_{\underline{ mn }}=\nabla_{a}g_{\underline{ mn }}+(\nabla_{\underline{ m }}a^{\underline{ k }})g_{\underline{ kn }}+(\nabla_{\underline{ n }}a^{\underline{ k }})g_{\underline{ mk }}
\end{aligned}
\end{equation}
$$
Speciálněji pro metrickou kovariantní derivaci ($\nabla g=0$)
$$
\begin{equation}
\begin{aligned}
\mathcal{L}_{a}g_{\underline{ mn }}=\nabla_{\underline{ m }}a_{\underline{ n }}+\nabla_{\underline{ n }}a_{\underline{ m }}
\end{aligned}
\end{equation}
$$
Což je ideální tvar na hledání killingových polí $a$ v $\mathcal{L}_{a}g_{\underline{ mn }}=0$:
##### Killingovy vektory s metrickou kovariantní derivací, bez torze
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}a_{\underline{ n }}+\nabla_{\underline{ n }}a_{\underline{ m }}\overset! =0
\end{aligned}
\end{equation}
$$

## Vztah $\nabla$ a $\mathrm{d}$
Připomenutí [vnější derivace](Vn%C4%9Bj%C5%A1%C3%AD%20kalkulus#D%20Vn%C4%9Bj%C5%A1%C3%AD%20derivace%20(u%C5%BE%20nutn%C4%9B%20na%20form%C3%A1ch)).
### Věta: $\mathrm{d}$ a $\nabla$
$$
\begin{equation}
\begin{aligned}
\mathrm{d}=\nabla{\wedge}+T\wedge\!\!\!\!{\cdot}\,,
\end{aligned}
\end{equation}
$$
kde 
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ m }}\wedge \omega_{\underline{ a_{1} }\dots \underline{ a_{p} }}:=\binom{p+1}{1}\nabla_{[\underline{ m }}\omega_{\underline{ a_{1} }\dots \underline{ a_{p} }]},
\end{aligned}
\end{equation}
$$
a 
$$
\begin{equation}
\begin{aligned}
(T{}\wedge\!\!\!\!{\cdot}~~\omega)_{\underline{ a_{0}a_{1} }\dots \underline{ a_{p} }}
&=T^{\underline{ n }}_\underline{ {a_{0}a_{1} }}\wedge \omega_{|n|\underline{ a_{2} }\dots \underline{ a_{p} }}\\
&:=\binom{p+1}{2}{T^{\underline{ n }}}_{[\underline{ a_{0}a_{1} }}~\omega_{|\underline{ n }|\underline{ a_{2} }\dots \underline{ a_{p} }]}
\end{aligned}
\end{equation}
$$

__Dk:__
Nejprve pro konexi nulovou (tj. i torze $=0$) pak efektivně $\nabla$ je $\partial$:
$$
\begin{equation}
\begin{aligned}
\mathrm{d}_{\underline{ a_{0} }}\omega_{\underline{ a_{1} }\dots \underline{ a_{p} }}=(p+1)\partial_{[\underline{ a_{0} }}~\omega_{\underline{ a_{1} }\dots \underline{ a_{p} }]}
\end{aligned}
\end{equation}
$$
zapneme $\Gamma$
$$
\begin{equation}
\begin{aligned}
\nabla_{\underline{ a_{0} }}\wedge \omega_{\underline{ a_{1} }\dots \underline{ a_{p} }}=\partial_{\underline{ a_{0} }}\omega_{\underline{ a_{1} }\dots \underline{ a_{p} }}&-{\Gamma^{\underline{ k }}}_{[\underline{ a_{0}a_{1} }}~\omega_{|\underline{ k }|\underline{ a_{2} }\dots \underline{ a_{p} }]}\\
&-{\Gamma^{\underline{ k }}}_{[\underline{ a_{0}a_{2} }}~\omega_{\underline{ a_{1} }|\underline{ k }|\underline{ a_{3} }\dots \underline{ a_{p} }]} - \dots,
\end{aligned}
\end{equation}
$$
členů bude celkem $p$. Víme, že antisymetrická část $\Gamma$ je právě $T$, čili zbude
$$
\begin{equation}
\begin{aligned}
=\mathrm{d}_{\underline{ a_{0} }}\omega_{\underline{ a_{1} }\dots \underline{ a_{p} }}-\binom{p+1}{2}{T^{\underline{ k }}}_{[\underline{ a_{0}a_{1} }}\omega_{|\underline{ k }|\underline{ a_{2} }\dots \underline{ a_{p} }]}
\end{aligned}
\end{equation}
$$

__Speciálně:__ 
- na fce $\mathrm{~d}_{\underline{ n }}f=\nabla_{\underline{ n }}f$
- na $1$-formy
$$
\begin{equation}
\begin{aligned}
\mathrm{d}_{\underline{ a }}\alpha_{\underline{ b }}=\nabla_{\underline{ a }}\alpha_{\underline{ b }}-\nabla_{\underline{ b }}\alpha_{\underline{ a }}+T^{\underline{ k }}_{\underline{ ab }}\alpha_{\underline{ k }}
\end{aligned}
\end{equation}
$$
- na $2$-formy
$$
\begin{equation}
\begin{aligned}
\mathrm{~d}_{a}\sigma_{kl}&=\nabla_{a}\sigma_{kl}+\nabla_{k}\sigma_{la}+\nabla_{l}\sigma_{ak}\\
&+T^{\underline{ n }}_{\underline{ ak }}\sigma_{\underline{ nl }}
+T^{\underline{ n }}_{\underline{ kl }}\sigma_{\underline{ na }}
+T^{\underline{ n }}_{\underline{ la }}\sigma_{\underline{ nk }}
\end{aligned}
\end{equation}
$$






