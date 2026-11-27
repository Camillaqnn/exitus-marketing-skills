---
name: gerador-prompts-imagem
metadata:
  version: "2.4"
description: "Conhecimento da Camilla sobre criação de imagem com IA — vocabulário técnico, mapa de ferramentas/modelos, metodologia das 6 camadas para prompt, specs oficiais e estrutura de prompt do Google para Nano Banana Pro (incluindo padrão de idioma por camada), histórico de prompts testados, biblioteca de 35 prompts do Nano Banana Pro, e módulos para consistência entre imagens de série/campanha (identity drift, LoRA, Omni Reference, brandbook, método Character Sheet para geração de personagem) e acabamento publicitário (causa técnica da 'cara de IA', vocabulário de luz/textura por ferramenta, pós-produção). Use SEMPRE ao escrever, revisar ou adaptar prompt de imagem, planejar série/campanha, escolher ferramenta de IA criativa, gerar/descrever um personagem, ou mencionar Nano Banana, Midjourney, Seedream, AXIS, mockup, upscale, iluminação, consistência de personagem/produto, brandbook, character sheet ou LoRA — mesmo sem pedir pela skill pelo nome."
---

# Gerador de prompts de imagem `v2.4`

<details>
<summary>Changelog da skill</summary>

- **v2.4** (2026-11) — Renomeada de `prisma` para `gerador-prompts-imagem`; título, descrição e menções internas a "Prisma" atualizados para o nome novo. Conteúdo técnico inalterado.
- **v2.3** (2026-08) — Funde os dois ramos independentes da v2.2. Adiciona `nano-banana-oficial.md`: specs técnicas atuais do Nano Banana Pro/2 (resolução, aspect ratios, limite de 14 imagens de referência, grounding com Google Search), a estrutura de prompt recomendada oficialmente pelo Google (5 elementos: sujeito, composição, ação, localização, estilo), as limitações que o próprio Google declara, e um padrão de idioma por camada do prompt (estrutura da cena em português, vocabulário técnico de câmera/luz/textura em inglês, texto renderizado na imagem na língua final entre aspas). Fontes: blog.google e Google Cloud Blog (nov/2025), mais pesquisa sobre uso real em português. A partir desta versão, todo prompt entregue à Camilla segue esse padrão de idioma por padrão — ver `Como aplicar`. Mantém o método Character Sheet (workflow Montag/Flora) adicionado à v2.2 anterior — ver abaixo.
- **v2.2** (2026-08) — Adiciona o método Character Sheet (workflow Montag/Flora) a `consistencia-serie.md`: as 3 etapas concretas de geração de personagem (foto-base neutra → prompt mestre da character sheet, incluído na íntegra → stills de campanha ancorados na sheet), com o porquê de cada bloco do prompt mestre. Adiciona gatilho para pedidos diretos de "gerar/descrever um personagem" sem exigir menção explícita a workflow ou campanha.
- **v2.1** (2026-08) — Enriquece os dois módulos v2.0 com material de fontes técnicas adicionais (blog Swipe da Human Academy, Aline Freitas): causa técnica da "cara de IA" (denoising, subsurface scattering, Vale da Estranheza), vocabulário de prompt em inglês por ferramenta (Midjourney `--style raw`/`--oref`/`--ow`, Nano Banana edição localizada, Seedream multi-referência), identity drift e suas 3 técnicas de mitigação (imagem-âncora, LoRA, geração em sequência), brandbook como sistema para IA, pós-produção manual de recorte/tom de pele, e um exemplo real de shooting de campanha em série (case iFood/AXIS).
- **v2.0** (2026-08) — Adiciona dois módulos: consistência entre imagens de uma série/campanha (`consistencia-serie.md`) e acabamento publicitário / anti-"cara de IA" (`acabamento-publicitario.md`). Renomeada de `prisma-image-prompt` para `prisma`.
- **v1.0** (2026-08) — Versão inicial: mapa de ferramentas (86 itens), glossário de fundamentos, metodologia das 6 camadas, biblioteca de 35 prompts do Nano Banana Pro, e histórico de prompts testados.

</details>

Esta skill reúne o material que a Camilla (designer na Exitus, ex-diretora de arte) trouxe de cursos/guias da Human Academy e Montag School sobre IA aplicada à criação visual. O objetivo é aplicar esse conhecimento automaticamente quando ela pedir ajuda com prompts de imagem, planejamento de série/campanha, ou escolha de ferramenta — sem que ela precise reanexar os PDFs de origem.

## Como decidir o que carregar

Carregue só o(s) arquivo(s) de referência relevantes ao pedido — não carregue tudo de uma vez.

| Pedido da Camilla | Arquivo a ler |
|---|---|
| "Qual IA eu uso pra X?" / dúvida sobre ferramenta, modelo ou hub | `references/mapa-ferramentas.md` |
| Pergunta sobre um termo técnico (o que é upscale, seed, inpainting, token, hub vs. plataforma etc.) | `references/fundamentos.md` |
| Pedido para **escrever ou revisar um único prompt** de geração/edição de imagem do zero | `references/estrutura-prompt.md` (metodologia) + `references/nano-banana-prompts.md` (se for edição via referência facial/produto) |
| Pedido de um prompt de uma categoria específica (retrato criativo, mockup, foto de produto, textura, iluminação, arquitetura, ângulo de câmera, upscale/restauração, conversão realismo↔ilustração) | `references/nano-banana-prompts.md` direto |
| Pedido envolvendo **várias imagens que precisam parecer da mesma série/campanha** — mesmo produto, mesmo personagem, ou mesma direção visual entre peças | `references/consistencia-serie.md` + `references/estrutura-prompt.md` |
| Pedido para **gerar/descrever um personagem** que vai ser usado em desdobramentos de campanha (mesmo que a Camilla só peça "a descrição do personagem", sem mencionar campanha) | `references/consistencia-serie.md` — seção "Método Character Sheet" |
| Dúvida sobre specs do Nano Banana Pro (resolução, aspect ratio, quantas imagens de referência aceita, "thinking mode", limitações atuais) ou sobre **em que idioma escrever o prompt** | `references/nano-banana-oficial.md` |
| Pedido que menciona explicitamente resultado "publicitário", "profissional", "sem parecer IA", still de produto de campanha, ou qualquer sinal de que o acabamento final importa tanto quanto a cena | `references/acabamento-publicitario.md` (usar em conjunto com o(s) arquivo(s) acima, não sozinho) |
| Pedido amplo/misto ("me ajuda a planejar uma campanha com IA", "que ferramentas preciso pra esse projeto") | `references/mapa-ferramentas.md` + `references/estrutura-prompt.md` |
| Antes de propor qualquer prompt novo, ou quando a Camilla pedir algo parecido com um uso anterior | `references/prompts-testados.md` (ver se já existe algo reaproveitável) |

## Como aplicar

**Ao escrever um prompt novo (peça única)**: sempre passar pelas 6 camadas (sujeito/ação, enquadramento/ângulo, luz/atmosfera, cenário/detalhe, estilo/referência, técnico — ver `estrutura-prompt.md`). Não entregar um prompt genérico ("uma pessoa numa sala bonita") — tomar as decisões de câmera, luz e estilo em vez de deixar em aberto. Se faltar contexto essencial (ex: não sabe se é still de produto ou retrato), perguntar objetivamente antes de escrever. Quando a ferramenta-alvo for especificamente Nano Banana Pro e o prompt usar mais de uma imagem de referência, conferir `nano-banana-oficial.md` — o Google exige declarar o papel de cada referência no prompt (qual imagem dá a pose, qual dá o estilo, qual dá o ambiente), e isso não é opcional para resultado consistente.

**Ao planejar uma série ou campanha (várias peças)**: antes de escrever os prompts individuais, decidir com a Camilla (ou inferir do contexto) qual mecanismo de consistência usar — imagem de referência (mais rápido), ficha técnica textual, ou LoRA (só se o personagem/marca for recorrente ao longo do tempo, não pontual) — ver `consistencia-serie.md`. Escrever uma vez o bloco de identidade e o bloco de direção visual da campanha, e reaproveitar esse texto em cada prompt individual da série, que continua passando pelas 6 camadas completas. Para séries longas (4+ peças) ou com múltiplos figurinos/contextos, considerar gerar por blocos com função definida para cada imagem (como no case iFood/AXIS documentado no arquivo) em vez de gerar tudo solto. Não tratar cada peça da série como um prompt isolado sem esse fio condutor.

**Ao pedir para gerar/descrever um personagem** (ex: "cria a descrição de um personagem para eu gerar", "preciso de um personagem para essa campanha"): seguir direto o método Character Sheet em `consistencia-serie.md` — não é preciso a Camilla mencionar "workflow" ou "Flora" para acionar isso. Reunir o essencial (perfil físico, se já há figurino definido, quantos ângulos cobrir) com no máximo as perguntas objetivas descritas no arquivo, entregar a Etapa 1 (foto-base neutra) pronta, e propor seguir para a Etapa 2 (prompt mestre da character sheet) assim que a foto-base existir — sem esperar pedido separado para cada etapa.

**Idioma do prompt (padrão a partir da v2.3)**: todo prompt entregue à Camilla para Nano Banana Pro/2 segue estrutura mista por padrão, sem precisar que ela peça — ver detalhamento e fontes em `nano-banana-oficial.md`: estrutura da cena (sujeito, ação, enquadramento, cenário) em português; vocabulário técnico de câmera/luz/textura (o bloco vindo de `acabamento-publicitario.md`) em inglês; texto que vai renderizar dentro da imagem escrito literalmente entre aspas na língua final desejada, independente da língua do resto do prompt. Não misturar tudo em português nem tudo em inglês por padrão — a mistura é intencional.

**Ao mirar acabamento publicitário**: reforçar a camada "Luz e atmosfera" com as 3 decisões de luz (origem, temperatura, fonte visível) antes de qualquer adjetivo, e vocabulário técnico de iluminação de estúdio (backlight, top light, split lighting, hard flash) — e a camada "Técnico" com pedido explícito de pós-produção (grão sutil, contraste editorial, nitidez seletiva), textura real nomeada (poros, fibra do tecido, imperfeições de material) e, quando a ferramenta aceitar, negativos explícitos (`no plastic skin`, `no CGI`) — ver `acabamento-publicitario.md`. Isso é um reforço sobre as 6 camadas, não uma metodologia à parte.

**Ao adaptar um prompt do Nano Banana**: os prompts em `nano-banana-prompts.md` são esqueletos — trocar produto/pessoa/marca pelos detalhes do pedido da Camilla, mantendo a estrutura técnica (não simplificar a especificidade). Se o pedido envolve uma foto de pessoa real, manter o padrão de abertura preservando identidade facial.

**Ao recomendar ferramenta**: usar a lógica "comece pelo problema, não pelo nome" do mapa — primeiro identificar o que ela precisa produzir, depois se é modelo/ferramenta/hub, depois se o resultado é rascunho ou entrega final. Responder com a recomendação e o "escolha quando" direto, sem listar todas as opções da categoria a menos que ela peça comparação. Prompts podem ser adaptados para múltiplas ferramentas (Nano Banana Pro, Midjourney, GPT Image) — a estrutura das 6 camadas funciona como prosa descritiva para qualquer uma delas; ajustar apenas idioma/sintaxe quando a ferramenta-alvo tiver convenção própria.

**Terminologia**: usar os termos exatos do glossário (ex: "image-to-image", "inpainting", "aspect ratio") em vez de paráfrases, já que é o vocabulário que a Camilla já aprendeu nesses cursos.

## Registrando prompts aprovados

Quando a Camilla indicar que um prompt funcionou bem (ex: "ficou ótimo", "salva esse", "guarda esse resultado"), adicionar uma entrada em `references/prompts-testados.md` seguindo o formato descrito no próprio arquivo — ferramenta usada, contexto, o prompt exato e o resultado observado. Isso não acontece automaticamente a cada prompt gerado — só quando ela sinalizar aprovação explícita. Não é preciso perguntar permissão para registrar; só confirmar brevemente que foi salvo.

## Fora do escopo desta skill

Perguntas gerais de design/direção de arte que não envolvem IA generativa seguem o conhecimento normal do Claude — não força aplicação deste material fora de contexto de geração/edição de imagem por IA.
