# Acabamento publicitário — fugir do "parece IA"

Este arquivo cobre o vocabulário e os cuidados que fazem uma imagem gerada parecer uma peça publicitária profissional em vez de uma geração de IA genérica. Usar como camada adicional sobre a metodologia de `estrutura-prompt.md` — não substitui as 6 camadas, reforça especificamente as camadas "Luz e atmosfera" e "Técnico" com o vocabulário abaixo.

Fonte: guia Montag "Criação de Imagens com IA" (seção Edição/O que evitar), biblioteca Nano Banana Pro da Human Academy, e artigos técnicos do blog Swipe (Human Academy) sobre textura de pele e iluminação natural em IA.

## Por que a imagem "parece IA" — a causa técnica

Não é acidente nem falta de sorte — é comportamento estrutural dos modelos, e saber a causa ajuda a escrever o prompt certo:

- **Pele lisa demais**: durante a geração (etapa de *denoising*), o modelo trata poros, sardas, linhas finas e variações de tom como "ruído" e os remove. Ele também foi treinado majoritariamente em bancos de imagem retocados e campanhas com filtro de beleza pesado — então "aprendeu" que pele ideal = superfície uniforme. Sem instrução em contrário, ele sempre volta a esse padrão.
- **Pele com aspecto de plástico/cera**: pele real não é opaca — a luz entra, se espalha pelas camadas internas e sai de novo (*subsurface scattering*). É um dos comportamentos mais difíceis dos modelos simularem. Sem isso, a pele fica opaca (aspecto de argila) ou com brilho superficial (aspecto de cera/plástico).
- **Luz "de estúdio invisível" mesmo em cena externa**: os modelos replicam o padrão mais comum do treinamento — luz frontal, uniforme, sem origem física clara, sombras suavizadas demais. O resultado tecnicamente "bonito" mas sem lógica de onde a luz vem.
- **Efeito "quase real" incomoda mais que estilizado**: fenômeno do Vale da Estranheza — quanto mais uma imagem se aproxima do real sem chegar lá, mais ela incomoda. Uma imagem que acerta olhos, luz e composição mas falha só na pele quebra a confiança do espectador mais do que uma ilustração assumidamente estilizada.

A implicação prática: humanizar uma imagem é ir na direção **oposta** do que o modelo faria sozinho — escolher parâmetros que reduzem o embelezamento automático, nomear a imperfeição, e dar à luz uma origem física reconhecível.

## As 3 decisões de luz antes de escrever qualquer prompt de iluminação

Trocar um adjetivo por outro ("beautiful lighting" → "cinematic lighting") não resolve nada — o modelo continua sem saber de onde vem a luz. Decidir sempre estas três coisas primeiro:

1. **Origem** — de onde a luz vem e como se comporta. Vago demais: "natural light". Funciona: `directional window light from the right`, `warm backlight creating rim light on the hair`.
2. **Temperatura** — quente, fria, neutra, dourada, azulada, fluorescente. Funciona: `warm tungsten lamp light`, `cold blue late-night light`, `warm editorial side light with soft shadows`.
3. **Fonte visível na cena** — uma janela, vela, luminária, tela de celular/laptop, letreiro, farol. Isso dá à cena uma lógica interna em vez de luz "vinda do nada". Funciona: `face lit by laptop glow, warm table lamp in the background`, `candlelight on the table as the main light source`.

## Vocabulário de luz de estúdio (as 4 configurações mais usadas)

Nomear a configuração pelo termo técnico dá resultado mais previsível do que descrever o efeito desejado:

- **Backlight** — luz principal atrás do sujeito, cria rim light (contorno luminoso) nas bordas, sujeito em silhueta parcial. Usar para separação forte do fundo.
- **Top light** — luz vinda de cima, enfatiza volume e textura superior, sombras descendentes. Usar para dramaticidade e peso visual.
- **Luz lateral / split lighting** — luz a ~90° do sujeito, divide claro/escuro nitidamente. Usar para tridimensionalidade e caráter.
- **Hard flash** — flash direto frontal, sombras duras atrás do sujeito, alto contraste, estética crua e direta. Usar para efeito documental/editorial jovem — não para still de produto clean.

Para still de produto "clean" (ex: perfume, eletrônico flutuando), a combinação padrão é: backlight forte + rim light sutil de contorno + sombras duras controladas + fundo em gradiente contínuo sem emenda.

## Vocabulário de textura real por material (contra o "plastic look")

Nomear a imperfeição natural em vez de só pedir "realista" — em inglês (a maioria dos modelos responde melhor):

**Pele** — base de realismo: `documentary-grade realism`, `natural skin tones`, `visible micro-details`. Textura: `visible skin pores`, `natural skin texture`, `unretouched photo`, `hyper-detailed skin`, `epidermal texture`. Microdetalhes: `peach fuzz`, `fine facial hair catching light`, `subtle freckles`. Aspecto: `natural skin oiliness`, `soft realistic sheen on the forehead`. Reflexo nos olhos (frequentemente esquecido): `natural eye reflections from the window light`, `subtle catchlights matching the room lighting`.

**Tecido**: trama das fibras visível, dobras realistas, micro-texturas, tecido estruturado, malha tecida.

**Metal**: leve granulação industrial, imperfeições naturais, micro riscos, escovado (não espelhado uniforme).

**Geral**: motion blur discreto onde faz sentido fisicamente, grão de filme fino, halation sutil, `subtle chromatic aberration` — em doses pequenas, quebram a limpeza artificial excessiva.

## Referências de câmera, lente e filme como direção técnica (não estética)

Citar um equipamento não é enfeite — aciona no modelo um repertório de contraste, profundidade de campo e comportamento de luz associado àquele equipamento real:

- `Canon EOS R6 II`, `Leica M11, 50mm f/2`, `Leica SL2`, `APO-Summicron 90mm f/2` — nitidez orgânica, transições suaves de luz.
- `85mm f/1.4` ou `f/1.8` — profundidade de campo rasa, separação suave sujeito/fundo. Uma lente macro força o modelo a um nível de detalhe que normalmente evitaria.
- `Kodak Portra 400` — tons de pele quentes, granulação natural, altas-luzes suaves.
- `film grain`, `slight film grain` — quebra a limpeza excessiva característica de IA.

## Prompts negativos — o que excluir explicitamente

Quando a ferramenta aceita negativos (ou mesmo em prosa, como restrição), nomear o que NÃO deve aparecer é tão eficaz quanto descrever o que deve:

- Pele: `no plastic skin`, `no airbrushed skin`, `no smoothing`, `no beauty filter`, `no AI glow`
- Render/CGI: `no 3d render`, `no CGI`, `no unreal engine`, `no octane render`, `no glossy skin`
- Exposição: `no overexposed highlights`

## Erros de prompt que geram "cara de render 3D" ou genérica

- **Adjetivo no lugar de direção**: `beautiful lighting`, `stunning light`, `cinematic glow` não dizem de onde vem a luz nem sua intensidade. Trocar por instrução concreta de origem/temperatura/fonte (ver seção acima).
- **Pedir qualidade demais**: `4k`, `8k`, `masterpiece`, `ultra-detailed` empurram para nitidez e brilho artificiais. Preferir referência concreta de câmera/lente/filme. Se precisar de mais resolução, fazer isso como etapa de upscale separada — depois de resolver luz e textura, não em vez disso.
- **Deixar saturação escapar**: `vibrant colors`, `highly saturated`, `bright` tendem a generalizar a cena. Preferir `muted tones`, `desaturated palette`, `organic color grading`, `soft natural contrast`.
- **Esquecer textura de pele**: sem instrução explícita, o modelo sempre lisa o rosto (ver seção de causa técnica acima).
- **Adjetivos empilhados e contraditórios**: "minimalista, ultra detalhado, caótico, limpo" cancela a si mesmo — o modelo volta à média.

## Ordem de camadas para retratos — luz entra antes de estilo

Em cenas com pessoas, a luz não deve ser tratada como acabamento no fim do prompt — ela deve vir logo no início, antes de roupa, cenário ou adjetivos de qualidade. Ordem recomendada: **fonte de luz → comportamento da luz → textura → câmera/filme → restrições negativas.**

Exemplo de prompt completo nessa ordem (referência de nível, não para copiar literalmente):
> portrait of a woman sitting near a window in a quiet apartment, soft window light from the left as the main light source, warm practical lamp in the background, muted tones, natural skin texture, subtle eye reflections from the window light, shot on Leica M11, 50mm f/2, Kodak Portra 400, slight film grain, realistic shadows, no plastic skin, no airbrushed skin, no 3d render, no CGI, no overexposed highlights

## Ajuste por ferramenta (o que muda entre Midjourney, Nano Banana e Seedream)

- **Midjourney**: tem tendência interna de embelezamento automático (pele lisa, sem "falhas"). O parâmetro **`--style raw`** desativa essa estilização e aproxima do realismo cru — não resolve sozinho, mas é o que abre caminho. Para naturalidade geral (não só pele): `--stylize` baixo favorece precisão sobre efeito bonito; `--chaos` alto aumenta variação e pode quebrar a lógica de luz — usar com cautela.
- **Nano Banana / Nano Banana Pro**: melhor para edição localizada por texto em vez de regerar tudo — ex. "remova as rugas sob os olhos, mas mantenha a textura dos poros nas bochechas". Também aceita imagem como referência de iluminação (não só de identidade): suba uma foto, frame de cinema ou pintura para indicar direção/dureza/temperatura da luz, sem copiar composição ou estilo.
- **Seedream**: resolve parte do problema na arquitetura do modelo (simula melhor subsurface scattering), então já parte de uma base mais realista. Ponto forte: edição que preserva rosto/pose/enquadramento ao corrigir só a luz — não precisa reger do zero. Aceita múltiplas imagens de referência simultâneas para ancorar textura de pele consistente entre gerações (ver também `consistencia-serie.md`).

## Pós-produção manual — recorte e correção de tom de pele (Photoshop)

Quando a imagem gerada precisa de acabamento manual fino antes da entrega:

**Recorte de cabelo/bordas complexas** (Select and Mask): visualização em `Overlay` (vermelho) ou `On Black`/`On White` para enxergar fios com clareza. Ferramenta: `Refine Edge Brush`, passar só nas bordas. Valores técnicos de referência: Radius 3–15px (Smart Radius ativado), Smooth 2–5, Feather 0.3–0.8, Contrast 10–25, Shift Edge -5% a -15% (negativo evita halo branco na borda).

**Limpeza de ruído na borda** ("poeira" — fios soltos, pixels transparentes, sujeira de recorte): via layer mask + brush suave (`Flow` 20–40%, `Hardness` 0–30%) — preto remove, branco revela.

**Correção de tom de pele**: pele clara tende a tons vermelho/rosado (podendo puxar a laranja); pele média fica em tons laranja/pêssego quentes-amarelados; pele escura tem mais profundidade em marrons, podendo puxar a vermelho ou a um tom frio esverdeado em sombras. Se a luz/cor do fundo "invadir" o tom de pele (ex. empurrar para magenta), usar os canais de cor da imagem para isolar e corrigir — uma paleta de referência real ajuda a calibrar o alvo.

## Checklist rápido — antes de considerar um prompt pronto

1. A luz tem origem, temperatura e (quando possível) fonte visível definidas — não é só um adjetivo?
2. A pele/material tem uma imperfeição nomeada (poro, fibra, risco) em vez de só "realista"?
3. Tem referência de câmera/lente/filme, em vez de só pedir "alta qualidade"?
4. Há prompts negativos explícitos quando a ferramenta aceita (`no plastic skin`, `no CGI` etc.)?
5. Não há adjetivos que se contradizem entre si na mesma descrição?
6. Se é still de produto: o fundo é descrito como gradiente contínuo sem emenda (não "fundo branco" genérico)?
7. Pulei alguma etapa de pós-produção (upscale, limpeza de borda, correção de tom) que a entrega final exige?

## Erros a evitar (do guia Montag, seção "O que evitar")

- Prompt vago — entrega a média do que foi pedido, que é sempre genérica.
- Adjetivos empilhados e contraditórios — cancelam-se entre si.
- Pedir tudo de uma vez — cena complexa pede as 6 camadas organizadas, não um parágrafo confuso.
- Ignorar a proporção/aspect ratio — gerar no formato errado custa refazer (e créditos) do zero.
- Confiar na primeira saída — ela é rascunho; iterar é o trabalho, não o desvio.
- Pular a pós-produção — sem upscale, limpeza e cor, a imagem entrega o "parece IA" de graça.

## Nota sobre idioma

Todo o vocabulário técnico de luz/textura/câmera deste arquivo é dado em inglês de propósito — é a língua em que esses termos foram consolidados no treinamento dos modelos, e onde o inglês faz diferença real de resultado. O resto do prompt (sujeito, ação, cenário) pode e deve ir em português. Ver `nano-banana-oficial.md` para o padrão completo de mistura de idioma por camada.
