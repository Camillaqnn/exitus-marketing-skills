---
name: "exitus-organizar-tarefa"
description: "Segunda etapa do fluxo de demandas de marketing do Exitus: pega o briefing já aprovado (etapa exitus-brifar-demanda) e organiza prazo, fluxo de etapas, responsável sugerido por etapa e prioridade, sempre pensando no Taskflow como destino final. Não acessa nenhuma ferramenta — só raciocina em cima dos dados fixos do time e do que a coordenadora de marketing informar, e devolve uma proposta de estrutura para ela validar antes da gravação."
---

# Organizar Tarefa (Exitus Marketing) — Etapa 2 de 3 (v1.2)

Segunda das três skills que substituem a `briefing-de-demandas`. Fluxo completo: **Brifar → Organizar → Confirmar e documentar**. Esta etapa recebe o briefing que a coordenadora de marketing (quem usa estas skills) já aprovou na `exitus-brifar-demanda` (colado na conversa ou referenciado por ela) e decide **como** essa demanda vai rodar no Taskflow: prazo de cada etapa, quem faz o quê, e a prioridade.

**Esta skill não tem acesso a nenhuma ferramenta.** Todo o raciocínio usa os dados fixos documentados aqui embaixo e o que a coordenadora disser na conversa. Isso é proposital: é a etapa que mais raciocina (calcula prazos, decide fluxo, sugere pessoas), então separar o raciocínio da gravação significa que um erro aqui vira um texto pra revisar, nunca uma task errada indo pro Taskflow sozinha.

## Changelog

- **v1.0** (setembro/2026) — Nasce do desmembramento da `briefing-de-demandas`. Herda o Passo 1 (fluxo/prazo/prioridade) e a lógica de prazos escalonados por etapa da skill antiga, sem o acesso a ferramentas que ela tinha — aqui é só planejamento. O destino de tudo isso é sempre o Taskflow (ver `exitus-confirmar-documentar` para o detalhe técnico de como ele é operado hoje).
- **v1.1** (setembro/2026) — O Taskflow ganhou integração própria conectada (deixou de ser "via Asana"), e isso trouxe três ajustes aqui: os nomes das duas últimas etapas do fluxo padrão mudaram para bater com o Taskflow real ("Validação" virou "Aprovação" e "Aprovado" virou "Validado" — mesma pessoa, mesmo papel, só o rótulo mudou); a prioridade agora tem 4 níveis (Baixa/Média/Alta/Urgente, o Taskflow tem um a mais que o Asana); e ficou explícito que o Taskflow não tem uma ferramenta de dependência real entre tarefas — a ordem entre etapas é garantida só pelos prazos escalonados que esta skill calcula, sem bloqueio automático do sistema (isso já era como esta skill raciocinava, só passou a estar documentado).
- **v1.2** (19/11/2026) — Atualização do time: não existe mais redatora, e as pessoas passam a ser citadas pelo papel, não pelo nome. Nova divisão do fluxo padrão: Redação, Revisão e Validado com a **coordenadora de marketing** (que planeja, escreve as copys, revisa e aprova a peça pra sprint), Direção de Arte com o **designer**, e Aprovação com o **diretor comercial** (aval final na sprint; nenhuma peça vai ao ar sem ele). Depois da aprovação, a coordenadora publica ou agenda. Prazos de produção alinhados ao POP-MKT-003 como referência de aviso.

## Quando usar

Depois que a coordenadora já rodou a `exitus-brifar-demanda` e está satisfeita com o briefing. Gatilhos: "organiza essa demanda", "bora ver prazo e responsável disso", ou ela colando o resultado da etapa anterior e pedindo pra seguir.

## O que pedir se faltar (nunca invente)

Se a coordenadora não tiver dado ainda, pergunte antes de montar a proposta:

- **Prazo final** da entrega (data ou data+hora). É sempre o prazo da etapa **Validado**; nunca invente — "essa semana" vira uma data explícita, mostrada pra ela confirmar. (O Taskflow só grava o dia, não a hora — se ela der um horário específico, guarde-o no texto da proposta mesmo assim, pra constar.)
- **Fluxo**: segue o padrão de 5 etapas (Redação → Direção de Arte → Revisão → Aprovação → Validado) ou é diferente dessa vez (pular etapa, rodar em paralelo, mais de um executor por papel)?
- **Prioridade**, se ela quiser decidir; senão, esta skill infere (ver abaixo) e avisa qual escolheu.

Se a demanda já veio com tudo isso (a coordenadora deu de uma vez), não pergunte de novo.

## Territórios naturais (sugestão, nunca decisão)

| Papel | Território natural | Papel no fluxo padrão |
|---|---|---|
| Coordenadora de marketing | Planejamento e pauta, redação, copy e textos em geral (com apoio das skills de ciclo), revisão de texto e aprovação da peça pra sprint | Redação e Revisão (e fechamento em Validado, publicando ou agendando) |
| Designer | Design e criação visual: artes, criativos, decks, apresentações | Direção de Arte |
| Diretor comercial | Aprovação final das entregas de marketing na sprint, e solicitante de boa parte das demandas (via sprint) | Aprovação (no fluxo padrão de marketing) |
| Executivo comercial | Comercial e contratos | Sem papel fixo; envolver se a demanda for de atendimento/leads |
| Automação e tecnologia | CRM, integrações, WhatsApp, implantação em escola | Sem papel fixo; envolver se a demanda for de pós-venda/implantação |

Território natural é ponto de partida, não reserva de mercado (Governança do Time de Marketing). Sempre proponha o responsável, nunca decida por conta própria — se a carga estiver concentrada numa pessoa, diga isso na proposta em vez de só empilhar mais uma etapa nela.

## Prioridade

Baixa / Média / Alta / Urgente. Se a coordenadora não especificar, infira pela urgência do prazo: menos de 24h tende a ser Urgente, menos de 48h tende a ser Alta, cerca de uma semana tende a ser Média, mais que isso tende a ser Baixa. Diga na proposta qual você escolheu, pra ela corrigir se estiver errado. Nunca deixe esse campo sem proposta.

## Fluxo padrão: 5 etapas em cadeia

Por padrão, toda demanda passa pelas 5 etapas abaixo, em ordem, cada uma só podendo começar quando a anterior termina:

| # | Etapa | Responsável sugerido | Natureza |
|---|---|---|---|
| 1 | Redação | Coordenadora de marketing | Criação |
| 2 | Direção de Arte | Designer | Criação |
| 3 | Revisão | Coordenadora de marketing | Conferência (revisão de texto e validação da peça pra sprint) |
| 4 | Aprovação | Diretor comercial | Conferência (aprovação final; nenhuma peça vai ao ar sem esse aval) |
| 5 | Validado | Coordenadora de marketing | Fechamento (publica ou agenda o conteúdo) |

Se a Aprovação pedir mudança, vale a regra do POP-MKT-003: **ajuste** (mudança pontual) volta pra Revisão; **refação** (desalinhamento de conceito) volta pra etapa anterior.

O Taskflow não bloqueia automaticamente a etapa seguinte enquanto a anterior não termina (diferente do que o Asana fazia com dependência real) — a ordem fica garantida só pelos prazos escalonados abaixo. Isso significa que alguém precisa atualizar manualmente o status da task principal e marcar a subtask da etapa concluída, pra próxima pessoa saber que pode começar.

Só use um fluxo diferente (pular etapa, trocar responsável, rodar em paralelo) se a coordenadora disser isso explicitamente. Se algum papel se repetir pra pessoas diferentes (ex: três revisores comerciais em paralelo), planeje uma etapa igual pra cada uma.

## Como distribuir os prazos entre as etapas (prazos escalonados)

As etapas **não herdam todas o mesmo prazo final** por padrão — cada uma recebe seu próprio prazo, calculado de trás pra frente a partir do prazo final. Redação e Direção de Arte (criação) recebem mais tempo; Revisão e Aprovação (conferência) recebem menos.

1. **Validado** = o prazo final exato que a coordenadora deu. Não recebe tempo adicional.
2. **Aprovação** = prazo de Validado menos 1 período. Um "período" é meio turno de trabalho (~4h): manhã (até 12h) ou tarde (até 18h, horário de Brasília).
3. **Revisão** = prazo de Aprovação menos 1 período.
4. **Redação** e **Direção de Arte** dividem igualmente o tempo restante entre agora (ou quando a demanda chegou, muitas vezes pela sprint com o diretor comercial) e o prazo de Revisão — Redação com a primeira metade, Direção de Arte com a segunda.
5. Se sobrar menos de meio dia útil pra Redação ou Direção de Arte, mantenha o rateio proporcional mesmo assim, mas avise na proposta que o prazo ficou apertado. Referência do POP-MKT-003 pra avisar: o designer precisa de 48h pra peça estática e 72h pra audiovisual na Direção de Arte, e o diretor comercial tem até 24h pra Aprovação.
6. Se a coordenadora der prazo específico pra alguma etapa, use o dela ali; as demais seguem a lógica acima, recalculada a partir do que ela deu.
7. No fluxo alternativo em paralelo (sem dependência entre etapas), todas podem receber o mesmo prazo final, a menos que a coordenadora dê prazos diferentes.

Exemplo: pedido numa segunda-feira, prazo final sexta-feira. Validado = sexta. Aprovação = quinta à tarde. Revisão = quinta de manhã. Redação ≈ 1,5 dia (segunda até terça à tarde), Direção de Arte ≈ 1,5 dia (terça à tarde até quinta de manhã).

## O que esta skill devolve

Uma proposta de estrutura, em texto, sem gravar nada no Taskflow:

- Fluxo escolhido (padrão de 5 etapas ou o combinado)
- Tabela: etapa → responsável sugerido → prazo calculado → depende de qual etapa anterior
- Prioridade escolhida (e o porquê, se foi inferida)
- Qualquer aviso de prazo apertado

## Entregando pra próxima etapa

Mostre a proposta pra coordenadora. Se ela topar (ou pedir ajuste, que você aplica e mostra de novo), o próximo passo é ela chamar a `exitus-confirmar-documentar`, levando o briefing da etapa 1 e esta estrutura da etapa 2. Esta skill não grava nada e não chama a próxima sozinha.

## Boas práticas

- Nunca decida responsável, só sugira — mesmo quando o território natural parece óbvio.
- Nunca inclua campos que o Taskflow já vai resolver nativamente (assignee, due date, status, prioridade) como texto solto no briefing — isso é problema da próxima etapa, não desta.
- Se o mesmo ajuste de prazo/território se repetir em várias demandas, proponha atualizar esta skill (`propose_skills`, `improvement`) em vez de reaplicar o ajuste de memória toda vez.