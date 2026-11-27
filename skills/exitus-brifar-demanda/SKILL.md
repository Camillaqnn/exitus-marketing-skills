---
name: "exitus-brifar-demanda"
description: "Primeira etapa do fluxo de demandas de marketing do Exitus: checa se a demanda já existe no Taskflow (integração própria conectada), entende o pedido e monta o briefing em 4 blocos (Descrição, Objetivo, O que deve conter, Entrega) usando o Brandbook Exitus do Drive como fonte. Não mexe em prazo, responsável nem grava nada — isso fica para as skills seguintes."
---

# Brifar Demanda (Exitus Marketing) — Etapa 1 de 3 (v1.4)

Esta é a primeira das três skills que substituem a antiga `briefing-de-demandas` (aposentada em setembro/2026). O fluxo completo é **Brifar → Organizar → Confirmar e documentar**, com a coordenadora de marketing (quem usa estas skills) olhando e validando o resultado de cada uma antes de mandar rodar a próxima — nenhuma skill chama a outra sozinha.

O trabalho desta etapa é só dois: garantir que a demanda é nova (não duplicada) e produzir um briefing de conteúdo fiel à marca. Ela **não** decide prazo, responsável, fluxo de etapas nem grava nada — isso é trabalho da `exitus-organizar-tarefa` e da `exitus-confirmar-documentar`.

## Changelog

- **v1.0** (setembro/2026) — Nasce do desmembramento da `briefing-de-demandas` em três skills separadas, pedido da coordenação de marketing depois de comparar o fluxo com a `GOVERNANCA-time-marketing.md` do Drive. Herda o Passo 0 (checagem de duplicidade) e o Passo 2–4 (contexto de marca, arquivo de referência, template de briefing em 4 blocos) da skill antiga; como fonte de marca passa a usar o Brandbook Exitus do Drive diretamente, em vez de só a skill `exitus-voz-e-marca`.
- **v1.1** (setembro/2026) — Corrigido: **tudo** roda no Taskflow, não só a gravação final. A checagem de duplicidade do Passo 0 (e qualquer leitura de tasks existentes) usa o Taskflow como sistema de referência, não o Asana tratado como algo à parte. Ver "Sobre a ferramenta" na skill `exitus-confirmar-documentar` para o detalhe técnico de como isso é executado hoje.
- **v1.2** (setembro/2026) — O Taskflow ganhou integração própria conectada aqui (conector `task-flow-marketing`). A checagem de duplicidade do Passo 0 agora usa essas ferramentas diretamente, e não mais o Asana como implementação provisória. A arquitetura também mudou: cada área da demanda agora é um projeto separado dentro do Taskflow (hoje: Marketing, Comercial, Eventos), em vez de um projeto único com a área entre colchetes no título. E os nomes das 5 etapas do fluxo padrão mudaram: Redação, Direção de Arte, Revisão, Aprovação e Validado (a lógica de responsáveis é a mesma de antes, só o nome de duas etapas mudou — ver `exitus-organizar-tarefa`).
- **v1.3** (setembro/2026) — Conferido contra a documentação técnica oficial do Taskflow: os resultados de `search`/`list_tasks` trazem o responsável atual da task (`assigneeName`) junto com nome e status, então o Passo 0 passa a mostrar isso também ao reportar uma possível duplicata pra coordenadora.
- **v1.4** (19/11/2026) — Atualização do time: não existe mais redatora no marketing, e as pessoas passam a ser citadas pelo papel, não pelo nome. Quem usa esta skill e valida cada etapa agora é a **coordenadora de marketing**, que também escreve as copys e revisa os textos. Nenhuma mudança no Passo 0 a 4.

## Quando usar

Gatilhos típicos: "precisamos de uma demanda sobre...", "preciso de um briefing para...", "vamos brifar isso...", ou qualquer pedido de peça/conteúdo de marketing descrito em linguagem natural. É sempre o primeiro passo do fluxo, mesmo quando a coordenadora já sabe prazo e responsável — isso só entra na etapa seguinte.

## Acesso

- **Taskflow**: somente leitura, só para checar duplicidade (Passo 0) e identificar o projeto certo pra área da demanda — via `search`, `list_tasks` e `list_projects`. Nunca cria, atualiza ou grava nada.
- **Google Drive**: somente leitura, para o Brandbook Exitus e para buscar arquivo de referência da demanda.

## Passo 0: Checar duplicidade no Taskflow

Antes de qualquer outra coisa, procure no Taskflow se já existe uma demanda igual ou muito parecida. Use `search` com os termos-chave do pedido (objeto da peça, área, cliente ou evento citado) — ela busca por texto em projetos e tarefas do workspace inteiro. Se já souber a área (e portanto o projeto), pode complementar com `list_tasks` filtrado por `projectId`. Busque por semelhança de sentido, não por string exata: um pedido de "arte pros aniversariantes de outubro" precisa encontrar uma task chamada "Arte de Aniversariantes do Mês".

Três respostas possíveis:

- **Não existe nada parecido**: siga para o Passo 1.
- **Existe a mesma demanda** (task aberta, ou concluída há pouco tempo, que já cobre exatamente isso): mostre o nome, o projeto (área), o responsável atual e o status atual pra coordenadora e pare por aí. Sugira continuar ou reabrir a demanda existente em vez de brifar uma nova.
- **Existe algo parecido, mas não é claramente a mesma coisa**: mostre o que encontrou (nome, projeto, responsável, status atual) e pergunte pra coordenadora: é a mesma demanda, é complementar a ela, ou é outra coisa? Só continue depois da resposta dela.

Não decida sozinho que é duplicata, a palavra final é sempre da coordenadora.

## Passo 1: Entender o pedido

Da descrição em linguagem natural da coordenadora, identifique o mínimo necessário pra brifar (prazo, responsável e prioridade **não** entram aqui, ficam pra próxima skill):

- **Área da demanda**: agora corresponde a um projeto real do Taskflow, não a um prefixo no título. Projetos existentes hoje: **Marketing**, **Comercial**, **Eventos**. Se a área citada pela coordenadora não bater com nenhum desses, avise: significa que um projeto novo precisa ser criado no Taskflow antes de gravar (isso só a `exitus-confirmar-documentar` pode fazer, por ter acesso de escrita) — confirme o nome da área com ela mesmo assim, pra não travar o briefing.
- **Nome curto da demanda**: o que é a peça, direto e específico (ex: "Arte de Aniversariantes do Mês", "Ajustes na apresentação institucional").
- **Contexto e motivo**: quem pediu, por que agora, o que motivou a demanda.
- **Público e formato**, se relevante pro conteúdo (rede social, apresentação, material impresso etc.).
- Qualquer especificação técnica já dada (dimensão, tipo de arquivo, orientação, plataforma).

**Nunca invente uma informação essencial que não foi dita.** Se a coordenadora já deu tudo isso no pedido original, não pergunte de novo, só pergunte o que realmente falta.

## Passo 2: Aplicar a marca (Brandbook Exitus)

Antes de escrever, leia o **Brandbook Exitus** no Drive, pasta "Brandbook" (dentro do Drive compartilhado de Marketing Exitus). Fonte principal: **"Brandbook Exitus - Versão 2"**, fileId `11nFIltFsyIy7bLOi19PBgF4OBBETx-b8SSalwh4fMH0`. Use como referência de posicionamento, valores, arquétipo, personalidade e público — não copie trechos literais, absorva a lógica.

Se esse fileId não abrir ou parecer desatualizado, procure de novo por `title contains 'Brandbook Exitus'` na pasta "Brandbook" e use a versão mais recente (maior número de versão e/ou `modifiedTime` mais novo). Não use cópias pessoais (títulos começando com "Cópia de") como fonte, elas podem estar em edição e divergir da versão oficial.

Depois de ler o Brandbook, complemente com a skill **`exitus-voz-e-marca`** para as regras práticas de escrita: vocabulário a preferir/evitar, frases proprietárias (usar exatamente como registradas, nunca parafrasear) e a fórmula obrigatória para falar de IA. Se o Brandbook e a skill divergirem em algum ponto pontual, o Brandbook (fonte mais recente e completa) prevalece — sinalize a divergência na resposta em vez de escolher em silêncio.

## Passo 3: Buscar arquivo de referência (Google Drive)

Sempre que a demanda tratar de algo que pode já existir como arquivo no Drive (uma apresentação, uma arte, um documento, um material que vai ser ajustado ou usado de base), procure automaticamente com `Google Drive:search_files` antes de fechar o briefing.

- Se encontrar: inclua o link direto no bloco Entrega do briefing (ou na Descrição, quando o próprio arquivo for o objeto do ajuste).
- **Isso não é um anexo de verdade no Taskflow.** Essa integração não tem uma ferramenta de anexar arquivo, só o link na descrição da task. Anexar de verdade (se o Taskflow tiver esse recurso na tela) é ação manual de quem abrir a task depois. Nunca diga que o arquivo já foi anexado, só que o link foi incluído.
- Se não encontrar: sem problema, siga normalmente, quem for executar cria o material do zero.

## Passo 4: Montar o briefing

### Nome de referência da demanda

Guarde a **área** (= qual projeto do Taskflow: Marketing, Comercial, Eventos ou outro) e o **nome da demanda** separadamente. O título real da task no Taskflow vai ser só o nome da demanda, sem colchetes, porque a área já fica implícita no projeto onde ela mora — isso é montado na etapa de confirmação (`exitus-confirmar-documentar`), esta skill não cria nada.

### Corpo do briefing

Template único, sempre nesta ordem e com esses rótulos exatos:

```
[Parágrafo de descrição: contexto geral, o que foi pedido e por quem, em tom corrido]

Objetivo:
[o que essa demanda busca alcançar]

O que deve conter:
- [ponto 1]
- [ponto 2]
- [ponto 3, etc.]

Entrega:
[o que precisa ser entregue, de forma direta e específica. Especificações técnicas entram aqui mesmo, sem bloco separado. Se achou arquivo de referência no Passo 3, o link entra aqui.]
```

Exemplo real (Aniversariantes do Mês):

```
O Gutemberg pediu uma arte para colocar na parede do nosso andar na empresa, celebrando quem faz aniversário naquele mês. A peça será trocada todo mês, conforme os aniversariantes mudam.

Objetivo:
Começar essa celebração como uma tradição da cultura da empresa

O que deve conter:
- Nomes dos aniversariantes do mês
- Layout pensado para ser atualizado facilmente todo mês
- Texto de celebração alinhado ao tom da marca

Entrega:
Arte para impressão e fixação na parede do andar, celebrando os aniversariantes do mês
```

Regras de escrita (padrão Exitus): nunca usar travessão ("—"), usar vírgula ou ponto final; destaque "Objetivo:", "O que deve conter:" e "Entrega:" (a próxima skill grava isso como texto simples na descrição da task — o Taskflow não tem um campo de HTML separado —, aqui basta manter os rótulos organizados).

## Entregando pra próxima etapa

Ao final, mostre pra coordenadora: a área e o nome da demanda, o briefing completo nos 4 blocos, e o arquivo de referência do Drive (se achou). Se ela topar, o próximo passo é ela chamar a `exitus-organizar-tarefa` colando ou referenciando esse resultado — esta skill não chama a próxima sozinha.

## Boas práticas

- "Entrega" deve ser específica e acionável: "post carrossel de 5 slides pro Instagram sobre X" é mais útil que só "post pro Instagram".
- Não negocie prazo, responsável ou fluxo de etapas aqui, mesmo que a coordenadora mencione de passagem — só anote e diga que isso vai ser resolvido na próxima etapa.
- Se o Brandbook mudar de forma relevante para como os briefings são escritos, proponha atualizar esta skill (`propose_skills`, `improvement`) em vez de aplicar a mudança só na memória da conversa.