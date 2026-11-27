# Nano Banana Pro — specs oficiais, estrutura de prompt do Google e idioma

Fonte: blog.google, "7 tips to get the most out of Nano Banana Pro" (Google DeepMind, nov/2025) e Google Cloud Blog, "Ultimate prompting guide for Nano Banana". Este arquivo cobre o que vem direto do fabricante — specs técnicas, a estrutura de prompt recomendada pelo próprio Google, e uma decisão prática sobre idioma do prompt. Usar em conjunto com `estrutura-prompt.md` (6 camadas, mais granular) e `acabamento-publicitario.md` (anti-"cara de IA"), não no lugar deles: a estrutura oficial do Google abaixo é o nível básico; as 6 camadas desta skill são o refinamento para quando a Camilla pedir nível profissional.

## O que é, hoje

Nano Banana Pro = **Gemini 3 Pro Image**. Tem "thinking mode": raciocina sobre o prompt antes de gerar, corrigindo erros de lógica de cena. Existe também o **Nano Banana 2** (Gemini 3.1 Flash Image) — mais rápido, voltado a edição conversacional e transferência de estilo, com menos "raciocínio" que o Pro. Ao recomendar entre os dois: Pro para cena complexa, texto na imagem, consistência de marca/personagem; Flash para iteração rápida e edições simples.

## Specs técnicas (checar sempre a doc oficial mais recente antes de prometer um número exato à Camilla — isso muda)

- **Resoluções nativas**: 1K, 2K, 4K.
- **Aspect ratios**: 1:1, 3:2, 2:3, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9, 21:9.
- **Referências de imagem**: até 14 imagens numa composição só (o número exato varia por superfície/plataforma — Gemini app, AI Studio, Vertex).
- **Grounding com busca**: o modelo conecta a dados reais do Google Search para gerar cenas/diagramas factualmente corretos (ex: um marco real num horário/estação específicos sai com geometria e luz verificadas, não inventadas).
- Toda geração carrega watermark invisível SynthID.

## Estrutura de prompt recomendada pelo Google (nível básico — "a visão")

5 elementos para estabelecer a cena:

1. **Sujeito** — quem/o quê, com especificidade real.
2. **Composição** — enquadramento (close extremo, plano aberto, contra-plongée, retrato).
3. **Ação** — o que está acontecendo, verbo concreto.
4. **Localização** — onde, com detalhe específico do ambiente.
5. **Estilo** — estética geral (3D, noir, aquarela, fotorrealista, still de produto anos 90).

Para **edição** (não geração do zero): instrução direta e específica ("troque a gravata do homem para verde"), não redescrever a cena inteira.

## Camada de refinamento oficial (equivalente ao que as 6 camadas desta skill já cobrem, mas vale ter os termos exatos do Google)

- **Composição e aspect ratio**: declarar o formato final ("pôster vertical 9:16", "plano cinemático 21:9").
- **Câmera e luz**: dirigir como diretor de fotografia (abertura, ângulo, temperatura de luz, color grading).
- **Texto na imagem**: declarar o texto exato e a tipografia desejada.
- **Restrição factual** (diagramas/infográficos): pedir precisão explicitamente.
- **Papel de cada referência**: com múltiplas imagens, dizer o que cada uma faz no prompt ("Imagem A = pose, Imagem B = estilo, Imagem C = ambiente") — isso é obrigatório em prompts com mais de uma referência, não opcional.

## Idioma do prompt — português ou inglês?

Não existe benchmark oficial do Google cravando "X% melhor numa língua". O que existe são sinais consistentes o bastante para virar padrão de entrega (aplicar sem precisar que a Camilla peça):

- **O modelo entende português e não recusa o idioma** — Gemini Image funciona em português, espanhol, japonês e praticamente qualquer língua que o Gemini suporte. Mas foi treinado majoritariamente em inglês, e relatos de uso real no Brasil mostram atrito real com português puro: comandos que precisam ser reformulados, ou que só funcionam depois de voltar pro inglês.
- **Onde o inglês faz diferença concreta**: vocabulário técnico de fotografia/cinema (`golden hour backlighting`, `shallow depth of field f/1.8`, `cinematic color grading`, o vocabulário de luz/textura de `acabamento-publicitario.md`). Esses termos estão ligados ao inglês nos bancos de imagem e legendas usados no treinamento — é o motivo de `acabamento-publicitario.md` já recomendar inglês para essa camada especificamente.
- **Onde o idioma importa menos**: a estrutura geral da cena (sujeito, ação, cenário, enquadramento). Com o thinking mode, o modelo interpreta bem português nessa camada — a diferença de qualidade aí vem mais da especificidade do prompt (as 6 camadas) do que da língua escolhida.
- **Onde português é necessário e funciona bem — texto renderizado na imagem**: o Google recomenda explicitamente escrever o prompt numa língua e declarar o idioma-alvo do texto de saída, com o texto desejado entre aspas. Se o texto final deve aparecer em português na peça, escrever ele em português entre aspas, independente da língua do resto do prompt — não traduzir mentalmente para inglês e torcer para a IA traduzir de volta.

**Padrão de entrega (regra prática a aplicar em todo prompt novo, sem precisar que a Camilla peça):**

| Camada do prompt | Idioma |
|---|---|
| Sujeito, ação, enquadramento, cenário (estrutura geral da cena) | Português |
| Vocabulário técnico de câmera, luz, textura, film stock | Inglês |
| Texto que vai aparecer renderizado dentro da imagem | Língua final desejada, sempre entre aspas |
| Prompts negativos (`no plastic skin`, `no CGI` etc., quando a ferramenta aceitar) | Inglês |

Não é uma regra rígida de tradução linha a linha — é sobre reconhecer qual pedaço do prompt se beneficia de qual idioma, mantendo o prompt coeso e legível como um todo.

## Limitações declaradas pelo Google (mencionar à Camilla quando relevante para gerenciar expectativa, especialmente em texto/diagrama/edição complexa)

- Texto pequeno, detalhes finos e ortografia podem falhar.
- Sempre conferir a precisão factual de diagramas/infográficos gerados — grounding reduz erro, não elimina.
- Geração multilíngue pode errar gramática ou perder nuance cultural.
- Edições complexas (blending de várias imagens, mudança de luz) podem gerar artefatos.
- Consistência de traços faciais entre edições sucessivas pode variar (ver `consistencia-serie.md` para mitigação).

## Quando usar este arquivo vs. os outros

- Dúvida sobre **specs** (resolução, aspect ratio, quantas referências aceita) → este arquivo.
- Dúvida sobre **idioma do prompt** → este arquivo.
- Escrever/revisar prompt do zero → `estrutura-prompt.md` (6 camadas) continua sendo a metodologia principal; usar a lista de 5 elementos daqui só como checagem rápida de que nada básico ficou de fora.
- Prompt não parecendo "profissional"/parecendo IA → `acabamento-publicitario.md`.
- Esqueleto pronto por categoria → `nano-banana-prompts.md`.
