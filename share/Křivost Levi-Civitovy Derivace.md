---
tags:
  - Magistr/Geomet/Geomet_I/téma
Téma: 20
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

# Křivost Levi-Civitovy derivace
Máme k dispozici metriku $g$, a k ni [metrickou](Klasifikace%20Kovariantn%C3%ADch%20Derivac%C3%AD#D%20Metrick%C3%A9) [L-C derivaci](Levi-Civitova%20Derivace#V%C4%9Bta%20Levi-Civitova%20derivace) $\nabla g=0$, $\mathrm{Tor}[\nabla]=0$, a tak umíme sundávat indexy $\to$ ${R_{abcd}}$.

### Symetrie Křivosti L-C
Platí tedy:
1) $R_{abcd}=R_{[ab]cd}=R_{ab[cd]}$
2) $R_{[abc]d}=0$ z [B1](K%C5%99ivost#I.%20Bianchiho%20Identity) a $R_{a[bcd]}=0$
3) $R_{abcd}=R_{cdab}$

Dk:
1) Použijeme, že L-C $\mathbf{R}_{ab}$ je metrická pseudoderivace vyjádřená pomoci [Riemannova tenzoru](K%C5%99ivost#D%20Riemann%C5%AFv%20tenzor):
$$
\begin{equation}
\begin{aligned}
0=\mathbf{R}_{ab}g_{cd}=-{{R_{ab}}^{m}}{}_{c}~g_{md}-{{R_{ab}}^{m}}{}_{d}~g_{cm}=-2{{R_{ab}}^{}}{}_{(cd)}.
\end{aligned}
\end{equation}
$$
2) Spočítejme
$$
\begin{equation}
\begin{aligned}
3R_{a[bcd]}&=R_{abcd}+R_{acbd}+R_{adbc}\\
&=\frac{1}{2}\left[R_{\underline{ abcd }}+R_{\underline{ badc }}+R_{\underline{ cadb }}+R_{\underline{ adbc }}+R_{\underline{ dacb }}\right]\\
&=\frac{1}{2}[{{-R_{\underline{ bcad }}}}-R_{\underline{ dbac }}-R_{\underline{ cdab }}]\\
&=\frac{3}{2} {{R_{[\underline{ bcd }]\underline{ a }}}} = 0
\end{aligned}
\end{equation}
$$

## Netriviální úžení 
[Ricciho tenzor](K%C5%99ivost#D%20Ricciho%20identity) je zde symetrický
$$
\begin{equation}
\begin{aligned}
\mathrm{Ric}_{\underline{ ab }} = \mathrm{Ric}_{(\underline{ ab })},
\end{aligned}
\end{equation}
$$
a to souvisí s antisymetričností v zadních dvou indexech
$$
\begin{equation}
\begin{aligned}
\text{Tr}R_{\underline{ ab }}=0.
\end{aligned}
\end{equation}
$$
Tuto vlastnost můžeme dosadit do [zúžených 2. Bianchiho identit](K%C5%99ivost#Z%C3%BA%C5%BEen%C3%A9%20B2%20b)), které zúžíme dále pomocí $g^{bc}$ kde bez Torze:
$$
\begin{equation}
\begin{aligned}
2\nabla_{[a}\mathrm{Ric}_{b]c}-\nabla_{m}{{R_{ab}}^{m}}{}_{c}=0 ~~/ g^{bc}\\
\implies\nabla^{c}\left(\mathrm{Ric}_{cn} -\frac{1}{2}\mathscr{R}g_{cn}\right)=0
\end{aligned}
\end{equation}
$$
Objekt uvnitř divergence je [Einsteinův tenzor](K%C5%99ivost#D%20Einstein%C5%AFv%20tenzor).

### Lemma: Dualita Riemannova tenzoru v $d=3$
Informaci antisymetrických tenzorů můžeme duálně kompaktifikovat, pro obě dvojice antisymetrických indexů:
$$
\begin{equation}
\begin{aligned}
{R_{\underline{ abcd}}} = \varepsilon_{\underline{ abk }}\varepsilon_{\underline{ cdl }}{{}^{*}R^{*}}^{\underline{ kl }}.
\end{aligned}
\end{equation}
$$
${{}^{*}R^{*}}^{\underline{ kl }}$ je již symetrický. 
A postupným stopováním (podle signatury metriky):
$$
\begin{equation}
\begin{aligned}
\mathrm{Ric}_{\underline{ ab }}= \mp ({{}^{*}R^{*}}_{\underline{ ab }}-\text{Tr}[{{}^{*}R^{*}}]g_{\underline{ ab }})
\end{aligned}
\end{equation}
$$
se dostáváme až na skalární křivost:
$$
\begin{equation}
\begin{aligned}
\mathscr{R}=\pm 2 ~\text{Tr}[{{}^{*}R^{*}}]
\end{aligned}
\end{equation}
$$
Ovšem při detailnějším rozboru si možná všimneme podobnosti s [Einsteinovým tenzorem](K%C5%99ivost#D%20Einstein%C5%AFv%20tenzor) a to ten, že 
$$
\begin{equation}
\begin{aligned}
\text{Ein}_{\underline{ ab }}=\mp~ {{}^{*}R^{*}}_{\underline{ ab }}.
\end{aligned}
\end{equation}
$$
Veškerá informace Křivosti je obsažena v [Einsteinově tenzoru](K%C5%99ivost#D%20Einstein%C5%AFv%20tenzor). A tedy na bezestopou část, [Weylův tenzor](K%C5%99ivost#D%20Weyl%C5%AFv%20tenzor), nic nezbude, tj. $C=0$. 
![[Podvarieta.svg|400]]

