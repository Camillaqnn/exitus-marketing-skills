# Consistência entre imagens de uma mesma série ou campanha

Este arquivo cobre o problema de gerar **várias imagens que precisam parecer parte do mesmo conjunto** — mesmo produto/personagem reconhecível e/ou mesma luz e paleta entre variações. É complementar a `estrutura-prompt.md` (que cobre como escrever um prompt individual bem construído): aqui o foco é o que se repete *entre* prompts de uma série.

Fonte: workflows da Human Academy (Character Sheet, Depth Map, Camera Angles), artigos técnicos do blog Swipe sobre consistência de personagem e brandbook em IA, e o case real de shooting de campanha iFood/AXIS.

## O problema tem nome: identity drift

Confiar só em variar palavras no prompt entre gerações não funciona além de poucas cenas — esse fenômeno é conhecido como **deriva de identidade (identity drift)**. Não é falha de prompt: acontece porque cada geração recomeça "do zero", reinterpretando o sujeito com pequenas diferenças a cada vez, influenciadas por composição, ângulo e luz da cena. Em 15–20 cenas, essas diferenças pequenas se acumulam até o sujeito ficar irreconhecível.

A pergunta certa antes de gerar qualquer série não é "qual ferramenta usar", mas **qual âncora estou fornecendo ao modelo em cada cena**. Sem âncora, nenhuma técnica abaixo compensa.

## Dois eixos de consistência (não confundir)

Uma série pode precisar manter:
1. **Identidade do sujeito/produto** — é a mesma pessoa, o mesmo objeto, com as mesmas proporções e características reconhecíveis em toda imagem.
2. **Direção visual da campanha** — mesma luz, paleta de cor, lente/ângulo, tratamento de pós-produção — mesmo que o sujeito ou cenário mude entre as peças.

Uma série publicitária normalmente precisa dos dois ao mesmo tempo. Ao planejar os prompts, tratar cada eixo separadamente e depois combinar — não misturar as duas preocupações num parágrafo só.

## Preparação: o "DNA do personagem" antes de gerar qualquer cena

Antes de abrir qualquer ferramenta, montar uma descrição estruturada do personagem/produto — termos vagos ("bonito", "estiloso") não ajudam porque o modelo só entende padrões, e descrição imprecisa vira imagem instável. Incluir: proporções do rosto, ângulo dos olhos, formato da mandíbula, textura e estilo de cabelo (para pessoa) ou forma, proporções, cor exata e textura de material (para produto).

A partir dessa descrição, gerar a **imagem âncora**: o sujeito em corpo inteiro, pose neutra, iluminação simples, fundo limpo — sem interferência de luz/sombra/cenário, só a referência pura do sujeito. Depois, gerar variações em diferentes ângulos (frente, perfil, três-quartos) — a chamada **folha de rotação** (equivalente à turnaround sheet da animação tradicional). Esse conjunto guia todas as gerações seguintes.

## Mecanismo 1 — Consistência via imagem de referência (mais rápido)

Usar quando já existe (ou pode-se gerar) uma imagem-base fixa do produto/pessoa.

- Anexar a imagem de referência e abrir o prompt reforçando preservação: para pessoas, usar o padrão já estabelecido em `nano-banana-prompts.md` ("use a foto anexa como referência facial exata, preserve identidade e estrutura facial, não altere as feições"). Para produtos, nomear o que não pode variar: forma, proporções, cor exata, textura de material, logotipo/gravações.
- **Nano Banana Pro / Nano Banana 2**: aceita até 14 imagens de referência simultâneas — alimentar com fotos reais do produto e do guia visual da marca ajuda o modelo a manter iluminação e ângulo coerentes com o objeto real. A edição conversacional multi-turn ("substitua o fundo por X, preserve 100% da embalagem e do rótulo") é mais confiável que regerar do zero.
- **Midjourney (V7+)**: usa **Omni Reference (`--oref`)** — evolução do antigo `--cref`, que agora considera não só rosto mas corpo, roupa e estilo. O parâmetro **Omni Weight (`--ow`)** controla o quanto a referência pesa: valores moderados funcionam melhor para gerar várias cenas diferentes; peso alto demais trava o personagem e a cena se adapta artificialmente a ele. Funciona tanto com imagens geradas na própria plataforma quanto com fotos externas reais.
- **Seedream**: permite múltiplas imagens de referência ao mesmo tempo — o modelo "aprende" volumetria, brilhos metálicos, textura e comportamento de cor sob ângulos diferentes, o que a ferramenta reporta como pontuação de consistência de até 9.6/10.
- **Truque para sequências longas**: em vez de usar uma única imagem como referência, reunir as variações da folha de rotação numa única composição (colagem mostrando frente/perfil/três-quartos) — isso dá ao modelo um "mapa visual" mais completo do sujeito, reduzindo erros em ângulos não cobertos por uma imagem isolada.

Cada prompt novo da série deve reanexar (ou referenciar explicitamente) a mesma imagem-base — não descrever o sujeito de memória a partir do prompt anterior, porque a descrição em texto perde detalhe a cada repetição.

## Mecanismo 2 — Consistência via ficha técnica textual

Usar quando não há uma imagem de referência fixa, ou como base para gerar a primeira imagem-âncora.

Montar uma **ficha de identidade** curta e reutilizável, escrita uma vez e colada no início de todo prompt da série:

```
FICHA: <nome do sujeito/produto>
- Aparência fixa: <cor exata, material, proporções, características distintivas>
- Não variar: <o que nunca muda entre as peças — ex: cor da embalagem, formato do rosto, tipo de tecido>
- Pode variar entre peças: <pose, cenário, ângulo, expressão — o que cada prompt individual decide>
```

**Cuidado com nomes de personagem**: se for nomear o sujeito no prompt (útil sobretudo em fluxos com LoRA), evitar nomes comuns ("Carlos", "Sofia") — o modelo já associa esses nomes a milhares de rostos diferentes, o que gera confusão. Preferir uma combinação única (ex: "KARVOX", "Zyla_v2").

**Cuidado**: texto puro é mais frágil que imagem de referência para manter identidade exata. Se a série for extensa (mais de 3–4 peças) ou o cliente for exigente com fidelidade, gerar a primeira imagem a partir da ficha e promovê-la a imagem-âncora para as seguintes (mecanismo 1), em vez de repetir só a ficha em texto por toda a série.

## Mecanismo 3 — Treinamento leve (LoRA), para personagens/marcas recorrentes

Quando o personagem ou produto vai aparecer em muitas peças ao longo do tempo (não uma série pontual), vale treinar um **LoRA** (Low-Rank Adaptation) — um pequeno modelo que ensina a ferramenta a reconhecer aquela identidade específica sem depender de referência a cada prompt.

- Precisa de 15–20 imagens do sujeito em ângulos, expressões e iluminações variadas (qualidade importa mais que quantidade — 5 imagens boas superam 20 aleatórias). Treinar só com um tipo de expressão/situação limita o resultado a esse contexto.
- Testar em situações extremas (luz diferente, ângulos difíceis) antes de confiar no LoRA para a série toda — se o resultado sair estranho, ajustar as imagens de treino, não tentar consertar via prompt.
- Plataformas com treinamento próprio: Freepik (Custom Characters), Krea AI, Leonardo.ai (fine-tuning). Depois de treinado, usa-se ali ou em ferramentas que aceitam LoRA externo (ComfyUI, Flux). Midjourney e Adobe Firefly não aceitam LoRA — nesses, usar o mecanismo 1 (referência).
- Para marca (não personagem): o mesmo princípio treina um LoRA com 20–50 imagens exclusivas do produto real, funcionando como "filtro de marca" que gera qualquer cenário novo sempre dentro da estética do produto original.

## Blindando a identidade visual da marca (brandbook como sistema, não referência solta)

Quando a série precisa seguir a identidade de uma marca (não só um personagem), formalizar o brandbook em texto ou arquivo antes de gerar qualquer prompt — o ganho não está em gerar bonito, está em **previsibilidade**. Uma estrutura de brandbook para IA cobre:

- **Essência e posicionamento**: papel da marca, territórios de atuação (ex: publicidade, food, lifestyle).
- **Princípios criativos**: o que rege a composição — ex. produto sempre como herói central da cena, cor como linguagem ativa (não decoração), gatilhos visuais intencionais (humor, exagero, desejo).
- **Identidade visual**: paleta com cores prioritárias/secundárias e regra de aplicação (ex: hex exatos, "uso obrigatório de fundo chapado para alto contraste"), tipografia oficial, diretrizes de direção de arte (tipo de luz preferido, tratamento de pele/textura).

Ferramentas para travar isso na prática:
- **Recraft**: aceita paleta por código hexadecimal exato — a geração fica limitada a esses canais cromáticos, sem vazamento de cor fora do branding.
- **Midjourney**: Style Reference (`--sref`) replica a estética exata de fotos institucionais homologadas, em vez de tentar descrever textura/luz/filtro em palavras.
- **Nano Banana Pro**: upload de múltiplas referências (produto real + guia visual) mais edição conversacional para ajustes pontuais sem perder consistência.

## Mantendo a direção visual da campanha (luz, paleta, lente)

Independente do mecanismo de identidade escolhido, a direção visual da campanha é um bloco à parte que se repete literalmente em todo prompt da série:

```
DIREÇÃO VISUAL DA CAMPANHA: <nome da campanha>
- Luz: <tipo, direção, temperatura de cor>
- Paleta: <cores dominantes e como se relacionam>
- Lente/tratamento: <ex: 85mm, profundidade de campo rasa, grão de filme sutil>
- Pós-produção: <o "look" final>
```

Esse bloco entra na camada "Estilo e referência" de cada prompt individual (ver `estrutura-prompt.md`) — não substitui as outras camadas, apenas fixa o que a camada de estilo deve dizer em toda peça da série.

## Ângulos de câmera como variável controlada

Quando a série precisa mostrar o mesmo sujeito/produto em ângulos diferentes, nomear o ângulo com vocabulário técnico exato reduz variância indesejada:

- **Full body / plano geral** — corpo inteiro visível, câmera em eixo frontal e altura neutra.
- **Close-up** — enquadramento fechado no rosto/detalhe, profundidade de campo rasa.
- **Extreme long shot / plano de conjunto** — sujeito pequeno no quadro, ênfase no ambiente.
- **High angle** — câmera acima do nível dos olhos, apontada para baixo.
- **Low angle / contra-plongée** — câmera abaixo do nível dos olhos, apontada para cima, reforça imponência.

Especificar não só o ângulo mas a lente equivalente e a distância aproximada da câmera é o que de fato mantém as proporções do sujeito coerentes entre os ângulos.

## Método Character Sheet (workflow Montag/Flora) — gerando a folha de rotação na prática

Este é o método concreto por trás da "imagem âncora + folha de rotação" descrita acima, documentado a partir do workflow Character Sheet da Montag School. Usar quando a Camilla pedir para **criar/gerar um personagem** que vai se desdobrar em várias peças de uma campanha — é o ponto de partida antes de qualquer still individual.

O processo tem 3 etapas, cada uma com sua função e seu tipo de prompt. Não pular direto para a etapa 3.

### Etapa 1 — Foto-base neutra (a "folha em branco" do personagem)

Antes de vestir o personagem com a roupa da campanha, gerar uma foto de estúdio neutra: corpo inteiro, roupa básica branca (tank top + short), fundo branco liso, pose parada. O objetivo é isolar identidade física (rosto, corpo, proporções) sem nenhuma interferência de styling ou luz de cena.

Fórmula do prompt (preencher as variáveis, manter a estrutura):

```
Full-body studio photograph of a [idade]-year-old [etnia] [gênero] with [descrição do cabelo: cor, comprimento, textura]. [Tipo de corpo] and approximately [altura]. Wearing a simple white cotton [peça neutra de cima] and [peça neutra de baixo]. [Pose e expressão]. [Descrição de pés/mãos se relevante]. The background is a seamless, brightly lit pure white studio backdrop.
```

Exemplo real do workflow:
> "Full-body studio photograph of a 40-year-old Latina woman with long, naturally wavy brown hair that flows past her shoulders. She has a slender, thin body type and is approximately 1.70m (5'7") tall. She is wearing a simple white cotton tank top and white cotton athletic shorts. She is standing confidently with her arms lightly crossed, smiling gently at the camera. Her feet are bare. The background is a seamless, brightly lit pure white studio backdrop."

Essa foto-base (e, quando a campanha exigir, o flatlay da roupa real fotografado separadamente) são as duas imagens de referência que alimentam a etapa 2.

### Etapa 2 — Prompt mestre da character sheet (o núcleo do método)

Com a foto-base (e o flatlay do figurino, se houver) como imagens de referência anexadas, este é o prompt que gera o grid de múltiplos ângulos/enquadramentos da mesma pessoa. Ele é longo de propósito — cada bloco existe para fechar uma porta específica por onde a IA costuma "vazar" identidade. Usar por completo, sem simplificar, mesmo que pareça repetitivo:

```
Generate a character sheet from the reference images, displaying the subject in multiple poses and framings against a clean white background. Character identity, wardrobe, and physical attributes must be preserved with absolute precision across every variation. The work is multi-angle reproduction, not reinterpretation.

Identity preservation: facial features, skin tone, hair (color, length, style, texture), body proportions, age, and ethnicity remain exactly consistent across all panels. Every pose must read as the same person — no drift in facial geometry, eye color, or distinguishing marks. Treat the reference as a locked character bible.

Wardrobe consistency: every garment, accessory, color, fabric, pattern, fit, and detail must match the reference exactly across all poses. Logos, prints, seams, buttons, jewelry, and footwear remain identical. No reinterpretation of fit, layering, or styling. Fabric behavior adjusts naturally to each pose (folds, tension, drape) while preserving identity.

Sheet layout: organize the panels in a clean grid against pure white background (#FFFFFF, no gradient, no texture).

Suggested composition:
Row 1 — Full body: front view, 3/4 front, profile (left), 3/4 back, full back.
Row 2 — Framings: medium shot (waist up), close-up (chest up), portrait (head and shoulders), detail shots (hands), detail shots (footwear).
Adjust the grid to the requested number of panels. Each panel separated by consistent negative space, no borders, no labels unless requested.

Lighting consistency: uniform, neutral studio lighting across every panel — soft key from front-left at ~45°, gentle fill, subtle rim for separation. Color temperature ~5400K. Same exposure, contrast, and shadow density in every variation. The character must appear lit by the same source in every panel.

Skin and material rendering: realistic macro detail with natural pores, subtle tonal variation, accurate subsurface scattering. Fabrics follow PBR behavior — visible weave, natural tension, realistic wear. Hard surfaces (accessories, footwear) show appropriate reflectance and edge definition. No plastic appearance, no oversmoothing.

Capture characteristics: medium-format sensor simulation (~14 stops dynamic range), 85mm equivalent lens, moderate depth of field with full subject sharpness. Optically resolved detail, minimal chromatic aberration, neutral color reproduction.

Color palette: faithful to reference — accurate skin tones, true wardrobe colors, no chromatic shift between panels. White background remains pure white without spilling color cast onto the subject.

Anatomical correction: explicitly resolve any inconsistency in faces, hands, fingers, joints, and proportions across every pose while preserving identity. Hands and feet rendered with full anatomical accuracy in every panel.

Final result: professional character reference sheet. Production-grade consistency across all variations, materially honest, optically resolved. The character reads as the same person in every panel, with maximum identity fidelity and zero AI artifacts in faces, hands, wardrobe, or geometry.
```

Por que cada bloco existe (para adaptar com critério, não só copiar):
- **"Multi-angle reproduction, not reinterpretation"** logo na abertura — trava o maior risco da tarefa antes mesmo de descrever o personagem: o modelo "recriar" a pessoa em vez de replicar.
- **Identity preservation + wardrobe consistency** — os dois eixos de consistência (sujeito e figurino) tratados como blocos separados, igual à lógica geral deste arquivo.
- **Sheet layout + composição sugerida** — o grid nomeado linha por linha é o que garante cobertura de ângulos suficiente para servir de referência depois (sem isso, o modelo escolhe ângulos aleatórios e pode faltar o 3/4 ou o close que a peça seguinte vai precisar).
- **Lighting consistency com valores técnicos** (~45°, ~5400K) — números, não adjetivos ("luz bonita"), é o que faz os painéis parecerem da mesma sessão de fotos.
- **Skin/material rendering + anatomical correction** — mitigação direta dos dois defeitos mais comuns de geração de pessoa por IA (pele plástica, mãos erradas); ver `acabamento-publicitario.md` para o porquê técnico disso.

### Etapa 3 — Stills de campanha a partir da character sheet

Uma vez aprovada, a character sheet passa a ser a imagem de referência para gerar cada still individual da campanha — cada still tem seu próprio prompt de still fotográfico (ver `estrutura-prompt.md` e `acabamento-publicitario.md`), mas agora ancorado numa identidade já travada, em vez de descrever a pessoa de novo a cada still.

## Gerando a descrição do personagem a partir de um pedido curto da Camilla

Quando a Camilla pedir a descrição de um personagem para gerar (sem ainda ter escrito a ficha completa), montar a **Etapa 1** acima a partir do que ela informar, seguindo esta ordem de perguntas objetivas — só perguntar o que for essencial e não tiver sido dito:

1. Perfil físico do personagem (idade aproximada, corpo, altura) — necessário para a fórmula da etapa 1.
2. O personagem já tem um propósito de campanha definido (para saber se o figurino da etapa 1 deve ser neutro puro, ou se já existe um flatlay de roupa real para anexar depois)?
3. Quantos ângulos/painéis a character sheet precisa cobrir — usar a composição sugerida (5 corpo inteiro + 5 enquadramentos) como padrão se a Camilla não especificar.

Depois de reunir isso, entregar a Etapa 1 pronta para gerar, e propor seguir direto para a Etapa 2 (prompt mestre) assim que a foto-base existir — não esperar a Camilla pedir cada etapa separadamente.

## Erros que quebram consistência mesmo com boa preparação

- **Trocas de roupa**: o modelo associa certos rostos a certos estilos visuais do treinamento — ao mudar de figurino (ex: uniforme → roupa casual), pode ajustar levemente o rosto para "combinar" com o novo contexto. Mitigar criando uma imagem-âncora separada para cada figurino importante da história, não só uma âncora geral.
- **Detalhes que aparecem do nada**: a IA completa a cena com base em contexto — um personagem em ambiente noturno urbano pode ganhar barba sem pedido, ou óculos escuros numa cena de verão. Repetir restrições explícitas nos prompts (ex: "sem barba", "sem acessórios") quando essas características importam.
- **Rosto que muda em planos abertos**: quando o rosto ocupa poucos pixels da imagem, o modelo simplifica detalhes — o personagem fica consistente em close mas "outra pessoa" em plano aberto. Onde possível, usar ferramentas que permitem refinar só a região do rosto após a geração.

## Exemplo real de shooting de campanha em série (case iFood/AXIS)

Estrutura usada para produzir uma biblioteca visual de marca com múltiplos personagens, contextos e modalidades sem perder unidade — modelo replicável para qualquer campanha com muitas variações:

1. **Defina os eixos de variação da campanha antes de gerar** (ex: no case, os eixos foram personagem, meio de entrega, categoria de compra, contexto de recebimento). Cada eixo muda o comportamento da cena, não só o objeto visível — ex. "farmácia" pede discrição e cuidado, "mercado" pede volume e praticidade; não basta trocar o conteúdo da sacola.
2. **Defina o núcleo visual antes de gerar por blocos**: look fotográfico principal, referências de personagem/roupa/ambiente/atmosfera — para não jogar tudo dentro de um prompt só e torcer para o modelo entender.
3. **Gere por blocos, não em lógica caótica**: separar a produção por família de imagem (ex: bloco de entregadores, bloco de clientes, bloco de cenas de apoio) mantém o trabalho controlável e permite avaliar consistência dentro de cada família antes de misturar.
4. **Toda variação nasce com uma função definida** — imagem principal com presença de marca, imagem horizontal para site, vertical para social, com espaço negativo para texto etc. Gerar volume sem função clara produz ruído, não biblioteca.
5. **Aplique três filtros depois da geração**: (a) coerência com a marca — a imagem pertence ao universo da marca em energia/paleta/comportamento?; (b) utilidade de campanha — serve para qual peça específica?; (c) continuidade visual — ela conversa com as outras da série?

## Checklist antes de gerar uma série

1. Já decidi qual mecanismo de identidade uso — referência de imagem, ficha textual, ou LoRA (se o uso for recorrente)?
2. A ficha de identidade e o bloco de direção visual estão escritos uma vez, prontos para colar em cada prompt?
3. Se a série tem múltiplos figurinos/contextos importantes, criei uma imagem-âncora para cada um, não só uma geral?
4. Cada prompt individual ainda passa pelas 6 camadas completas — a ficha/direção visual não substitui isso, só fixa duas das camadas?
5. Defini os eixos de variação da campanha e vou gerar por blocos, com função clara para cada peça?
6. Se a série é longa (4+ peças) e a fidelidade importa, considerei promover a primeira geração aprovada a imagem de referência para as seguintes?
