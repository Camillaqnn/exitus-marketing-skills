---
name: "exitus-confirmar-documentar"
description: "Terceira e última etapa do fluxo de demandas de marketing do Exitus: junta o briefing (exitus-brifar-demanda) e a estrutura (exitus-organizar-tarefa), mostra a proposta completa pra coordenadora de marketing confirmar, e só depois cria e ajusta a task e as subtasks no Taskflow — o único lugar onde tudo isso vive, com integração própria conectada aqui."
---

# Confirmar e Documentar (Exitus Marketing) — Etapa 3 de 3 (v2.3)

Última das três skills que substituem a `briefing-de-demandas`. Recebe o briefing aprovado (`exitus-brifar-demanda`) e a estrutura aprovada (`exitus-organizar-tarefa`), monta a proposta final, espera confirmação da coordenadora de marketing (quem usa estas skills), e só então cria e ajusta a demanda no Taskflow.

## Sobre a ferramenta: tudo vive no Taskflow

**Taskflow é o único lugar onde uma demanda existe de verdade** — a checagem de duplicidade (etapa 1), a criação e qualquer ajuste posterior (esta etapa) apontam pro mesmo sistema. Desde setembro/2026 o Taskflow tem uma integração própria conectada aqui (conector `task-flow-marketing`), então as chamadas técnicas das três skills usam essas ferramentas diretamente — não é mais necessário passar pelo Asana como implementação provisória. Nesta skill, os nomes de ferramenta aparecem como `Taskflow:nome_da_ferramenta` (por exemplo `Taskflow:create_task`), que corresponde a `task-flow-marketing:create_task` na lista de ferramentas.

O modelo de dados do Taskflow é mais simples do que o do Asana era: não existe "seção" (coluna de quadro), nem campo customizado, nem dependência real entre tarefas. O que existe:

- **Projeto** = a área da demanda (Marketing, Comercial, Eventos, e outras que forem criadas).
- **Task principal** = a demanda, com um campo de status (usado aqui pra guardar o nome da etapa atual, sempre espelhando a subtask em andamento — ver seção Status), prioridade nativa (Baixa/Média/Alta/Urgente) e um prazo (`dueDate`, só data, sem horário).
- **Subtask** = uma etapa do fluxo, com responsável, prazo e prioridade próprios, mas só dois status possíveis: Pendente ou Concluído.
- **Sem dependência automática**: nada bloqueia a etapa seguinte enquanto a anterior não termina, e o Taskflow não muda o status da task sozinho quando uma subtask é concluída. A ordem é garantida só pelos prazos escalonados que a `exitus-organizar-tarefa` calcula; avançar de etapa é sempre uma atualização manual, feita em par (`Taskflow:update_subtask` marcando a etapa concluída + `Taskflow:update_task` levando o status da task pra próxima subtask).

Isso tudo foi confirmado contra a documentação técnica oficial do Taskflow (levantamento de código-fonte de setembro/2026): as 11 ferramentas MCP disponíveis são exatamente as que esta skill já usa, sem tool de exclusão, anexo, comentário, convite/membro, status/tag personalizado ou duplicação de task. Coisas específicas que valem registrar:

- **Não existe nenhuma ferramenta pra descobrir o ID de uma pessoa pelo nome.** As ferramentas do Taskflow só conseguem atribuir uma task a um `assigneeId` que a conversa já conhece por algum outro meio — nunca "escolhendo" um membro pelo nome. Os IDs fixos na tabela de Pessoas abaixo são a única fonte confiável; ver Boas práticas pra como confirmar o ID de alguém novo.
- **O status da task principal precisa bater exatamente (inclusive maiúsculas/minúsculas e acentos) com uma das opções do seletor de status do Taskflow.** A API aceita qualquer texto, mas se o valor gravado não for idêntico a uma opção existente, a tela mostra "Backlog" (a primeira da lista) no lugar. Use sempre os valores da seção Status abaixo, copiados exatamente.
- **O gráfico "Distribuição por Status" do Dashboard do Taskflow usa uma lista fixa de status padrão do sistema**, diferente do Kanban e das Listas (que mostram qualquer status em uso, incluindo os nossos). Os nomes de etapa desta skill aparecem no Kanban e nas listas, mas não nesse gráfico específico do Dashboard — não é um erro desta skill, é uma limitação conhecida do Taskflow.

## Changelog

- **v1.0** (setembro/2026) — Nasce do desmembramento da `briefing-de-demandas`. Herda o Passo 5 ("Antes de gravar: proponha e confirme" e o passo a passo de criação) e o Passo 6 (confirmação pós-criação). É a única das três skills novas com acesso de escrita a alguma ferramenta.
- **v1.1** (setembro/2026) — Corrigido: o Taskflow é o destino único de tudo (checagem, criação e ajustes), não só da gravação final. Reescrita a seção "Sobre a ferramenta" pra deixar claro que o Asana era só a implementação técnica provisória do Taskflow, usada pelas três skills, e não um sistema à parte que seria substituído depois.
- **v2.0** (setembro/2026) — O Taskflow ganhou integração própria conectada aqui, substituindo o Asana como implementação. Reescrita completa dos dados fixos e do passo a passo de gravação pro modelo real do Taskflow: projetos por área (Marketing, Comercial, Eventos) no lugar de um projeto único com seções, prioridade nativa de 4 níveis no lugar de custom field, status de texto livre na task principal no lugar de seção + campo de status separado, subtasks só com Pendente/Concluído, e sem ferramenta de dependência real (removido o passo de encadear dependências e o de limpar seguidores, que não existem nesse conector). Nomes das 5 etapas atualizados: Redação, Direção de Arte, Revisão, Aprovação, Validado.
- **v2.1** (setembro/2026) — Conferido tudo contra a documentação técnica oficial do Taskflow (levantamento do código-fonte). Nenhuma mudança de fundo: as 11 ferramentas, os campos e os limites já documentados aqui batiam com o real. Adicionado: aviso sobre o gap conhecido do gráfico "Distribuição por Status" do Dashboard, procedimento pra confirmar o ID de uma pessoa nova no time (já que não existe busca de pessoa por nome), e o aviso de que `projectId` de uma task é imutável (não dá pra "mover" uma demanda de projeto via `update_task`, só recriar no projeto certo).
- **v2.2** (setembro/2026) — Três ajustes pedidos pela coordenação de marketing na criação da demanda "Arte de Dia do Professor para Instagram": (1) regra explícita de que o status da task principal **sempre segue as subtasks**: na criação vai pra primeira subtask, e a cada subtask concluída avança pra próxima subtask pendente, sempre no mesmo movimento; (2) o valor de status da primeira etapa é `redação`, com "r" minúsculo, como está no seletor do Taskflow (gravar "Redação" fazia a tela mostrar "Backlog"), e o status precisa bater exatamente com a opção do seletor; (3) registrados os IDs técnicos de projetos e pessoas confirmados pelo `assigneeName` devolvido.
- **v2.3** (19/11/2026) — Atualização do time: não existe mais redatora, e as pessoas passam a ser citadas pelo papel, não pelo nome. Tabelas de Projetos e Pessoas reescritas por papel: Redação, Revisão e Validado com a **coordenadora de marketing** (quem usa a skill e é a responsável geral da task), Direção de Arte com o **designer**, Aprovação com o **diretor comercial** (aval final na sprint; depois disso a coordenadora publica ou agenda). Os IDs de quem saiu do time foram removidos; os IDs de coordenadora, designer e diretor comercial ficam a confirmar pelo procedimento de pessoa nova. Reabertura de etapa alinhada ao POP-MKT-003 (ajuste volta pra Revisão, refação volta pra etapa anterior).

## Quando usar

Depois que a coordenadora já rodou `exitus-brifar-demanda` e `exitus-organizar-tarefa` e está satisfeita com os dois resultados. Também é esta skill que trata **ajustes** numa demanda já existente no Taskflow (mudar prazo, responsável, status, concluir etapa etc.), já que é a única com acesso de escrita. Gatilhos: "pode criar", "documenta isso", "bora fechar essa demanda", "ajusta o prazo dessa task", "terminei a redação", ou ela colando os dois resultados anteriores.

## Acesso

- **Taskflow**: leitura e escrita, via conector `task-flow-marketing`: `create_task`, `create_subtask`, `update_task`, `update_subtask`, `get_task`, `list_tasks`, `list_subtasks`, `list_projects`, `create_project` (só quando precisar abrir uma área nova), `search`.

## Dados fixos do Taskflow

- **Workspace**: "Marketing Exitus" — resolvido automaticamente pela integração a partir de quem está autenticado, nunca precisa ser informado ou mostrado.

### Projetos (= área da demanda)

| Área / Projeto | Membros hoje | ID técnico (uso interno) |
|---|---|---|
| Marketing | Coordenadora de marketing, designer, diretor comercial | `proj_1790109254330` |
| Comercial | Coordenadora de marketing, designer, diretor comercial, executivo comercial, automação e tecnologia | `proj_1790109231180` |
| Eventos | Coordenadora de marketing, designer, diretor comercial | `proj_1790109244942` |

Os IDs de projeto são só de uso técnico desta skill (nunca mostre um ID técnico pra coordenadora). Confira com `Taskflow:list_projects` se ainda batem antes de usar. Se a área da demanda não corresponder a nenhum projeto existente, confirme o nome com a coordenadora e use `Taskflow:create_project` pra criar um novo antes de seguir (ver Passo 2) — dá pra já popular `memberIds` com quem for participar, se a coordenadora disser quem.

**`projectId` é imutável depois de criada a task** (a própria API não aceita mudar isso via `update_task`). Se a coordenadora pedir pra "mover" uma demanda de área/projeto, avise que isso não existe como ajuste — a alternativa é recriar a task no projeto certo, o que tem o mesmo custo de criar uma demanda nova.

### Pessoas

| Papel | Papel no fluxo padrão | ID técnico (uso interno) |
|---|---|---|
| Coordenadora de marketing | Redação, Revisão, Validado (publica ou agenda depois da aprovação) | a confirmar |
| Designer | Direção de Arte | a confirmar |
| Diretor comercial | Aprovação (aval final na sprint) | a confirmar |
| Executivo comercial | Sem papel fixo (só no projeto Comercial) | um de `0Cg15hirCXMvSTrHMrH5ptBymw62` / `YAaV17q3QYSVRFcDCG6BifqnsPB2` (não confirmado qual) |
| Automação e tecnologia | Sem papel fixo (só no projeto Comercial) | o outro dos dois acima (não confirmado qual) |

Os IDs de cada pessoa ficam só no uso técnico desta skill, nunca aparecem pra coordenadora na conversa. Pra coordenadora, designer e diretor comercial, enquanto o ID estiver "a confirmar", siga o procedimento de pessoa nova (ver Boas práticas) antes da primeira atribuição. Pra executivo comercial e automação e tecnologia, antes da primeira atribuição, confirme qual ID é de quem conferindo o `assigneeName` devolvido pela criação (se vier a pessoa errada, corrija na hora com `update_subtask`/`update_task` e avise a coordenadora).

### Prioridade

`Baixa`, `Média`, `Alta` ou `Urgente` — string nativa no parâmetro `priority` de task e subtask, sem custom field e sem precisar montar JSON (isso era só necessidade do Asana).

### Status

**Regra principal: o status da task principal sempre segue as subtasks.** Ele mostra a etapa em que a demanda está, que é sempre a primeira subtask ainda `Pendente` na ordem do fluxo combinado.

- **Na criação:** o status da task vai para a etapa da **primeira subtask** do fluxo. Se a primeira subtask é REDAÇÃO, o status é `redação`; se o fluxo combinado pular a Redação e começar pela Direção de Arte, o status é `Direção de Arte`, e assim por diante.
- **Ao concluir uma etapa:** no mesmo movimento, marque a subtask como `Concluído` (`Taskflow:update_subtask`) e mude o status da task para a etapa da **próxima subtask pendente** (`Taskflow:update_task`). Nunca faça só uma das duas coisas.
- **Ao reabrir uma etapa** (voltar uma subtask para `Pendente` quando o diretor comercial, na Aprovação, pede mudança): **ajuste** (mudança pontual) reabre a Revisão; **refação** (desalinhamento de conceito) reabre a etapa anterior. O status da task volta para a etapa reaberta, já que ela passa a ser a primeira pendente.
- **Quando todas as subtasks estiverem concluídas:** pergunte pra coordenadora se muda o status para `Concluído` (opção que existe no seletor); não decida sozinho.
- Antes de mudar o status num ajuste, confira as subtasks com `Taskflow:list_subtasks` pra saber qual é a primeira pendente, em vez de supor.

**Valor exato de status por subtask.** O valor gravado precisa ser **idêntico** a uma opção do seletor de status do Taskflow, senão a tela mostra "Backlog":

| Subtask (título) | Status da task principal (valor exato) |
|---|---|
| REDAÇÃO | `redação` (com "r" minúsculo) |
| DIREÇÃO DE ARTE | `Direção de Arte` |
| REVISÃO | `Revisão` |
| APROVAÇÃO | `Aprovação` |
| VALIDADO | `Validado` |

Se a coordenadora renomear ou criar opções no seletor, peça um print da lista e atualize esta skill com os valores novos. Se o fluxo combinado tiver uma etapa que não existe no seletor, avise a coordenadora antes de gravar (ela pode criar a opção em "+ Criar novo status...").

- **Subtask**: só `Pendente` ou `Concluído`. Crie sempre como `Pendente`; marque `Concluído` quando a etapa terminar (`Taskflow:update_subtask`), sempre junto com o avanço do status da task.

### Prazo

`dueDate` é só uma data (sem horário). Se a coordenadora der um horário específico ("até as 17h"), guarde esse detalhe no texto da descrição ou avise que o Taskflow só grava o dia, não a hora exata.

## Passo 1: Antes de gravar, proponha e confirme

Junte o que veio das duas etapas anteriores e monte a proposta final:

- Área (projeto) e nome da demanda
- Briefing inteiro (os 4 blocos, sem resumir)
- Prioridade e prazo final
- Tabela de etapas: nome, responsável, prazo
- Status inicial da task (= etapa da primeira subtask)
- Arquivo de referência do Drive, se algum foi encontrado na etapa 1

Mostre tudo isso pra coordenadora e espere a confirmação ("pode criar", "tá bom", "manda ver", "confirmar" já bastam). **Única exceção:** se ela já disse explicitamente pra criar direto ("cria e grava", "pode criar direto", "sem precisar confirmar"), pule a espera e vá pro Passo 2. Se ela pedir ajuste, ajuste e mostre de novo antes de gravar — nunca grave a versão antiga. A mesma lógica de propor-e-confirmar vale para **ajustes** numa demanda já existente, não só pra criação nova.

## Passo 2: Gravar no Taskflow

1. **Confirme o projeto da área.** Se a área bater com Marketing/Comercial/Eventos (ou outro projeto que já exista), use o `projectId` dele. Se não existir, proponha criar um projeto novo com `Taskflow:create_project` (nome = área) e só crie depois que a coordenadora confirmar.
2. **Crie a task principal** com `Taskflow:create_task`: `projectId`, `title` = nome da demanda (sem colchetes de área, já que a área é o projeto), `description` = briefing completo em texto simples (mantenha os rótulos "Objetivo:", "O que deve conter:", "Entrega:" como estão, sem tags HTML — o Taskflow não tem campo de rich text; se houver horários por etapa, acrescente uma linha "Prazos por etapa (horários de referência)" no fim), `assigneeId` = responsável geral (coordenadora de marketing, salvo combinado outro), `priority`, `startDate` = data de hoje, `dueDate` = prazo final (etapa Validado), `status` = valor exato da etapa da **primeira subtask** do fluxo combinado (normalmente `redação`, com "r" minúsculo — ver tabela na seção Status).
3. **Crie uma subtask por etapa** com `Taskflow:create_subtask`, na ordem do fluxo: `taskId` = id da task principal, `title` = nome da etapa em caixa alta (ex: "REDAÇÃO", "DIREÇÃO DE ARTE"), `assigneeId`, `dueDate` conforme a etapa 2 calculou, `priority` (herdada da task principal), `status` = "Pendente". Repita pra cada etapa do fluxo combinado. Confira o `assigneeName` devolvido em cada uma.
4. **Confira o status.** Depois de criar as subtasks, confirme que o status da task corresponde à primeira subtask criada; se não corresponder, corrija com `Taskflow:update_task`.
5. **Sem passo de dependência.** O Taskflow não tem uma ferramenta de bloqueio entre tarefas — a ordem fica garantida só pelos prazos escalonados. Não tente simular isso com tags ou texto, só confie nos prazos e no acompanhamento manual.
6. **Sem limpeza de seguidores.** O Taskflow (por essa integração) não expõe esse conceito, não tem o que limpar.
7. Se faltar alguma etapa no fluxo combinado, simplesmente não crie a subtask correspondente (e o status inicial passa a ser a primeira subtask que existir).

**Ajustes numa demanda já existente** (mudar prazo, responsável, status, concluir etapa etc.) seguem o mesmo Passo 1 (propor e confirmar) e depois usam `Taskflow:update_task` (task principal) ou `Taskflow:update_subtask` (uma etapa específica) nos campos relevantes. Use `Taskflow:search`, `list_tasks` ou `list_subtasks` pra achar a task/subtask certa antes de atualizar. Informe pelo menos um campo pra mudar (a ferramenta recusa update sem nenhum campo). Ao replanejar um prazo, diga qual era o prazo anterior antes de trocar. Sempre que uma subtask mudar de status, atualize o status da task principal conforme a regra da seção Status, com o valor exato da tabela.

**Se uma gravação falhar:** mostre o erro e o que estava tentando gravar. Não tente de novo em silêncio com valores diferentes.

## Passo 3: Confirmar a criação ou o ajuste

Depois de gravar, mostre de forma direta: o título e o projeto/área (criado ou ajustado), o status atual da task, a cadeia de etapas (quem faz o quê, em que prazo), e o status de cada subtask. O Taskflow (por essa integração) não devolve um link direto pra task — diga pra coordenadora procurar pelo nome dentro do projeto certo. Não precisa repetir o briefing inteiro de novo, o conteúdo completo já foi mostrado na proposta do Passo 1.

## Boas práticas

- Sempre proponha antes de gravar (Passo 1), seja criação ou ajuste — única exceção é a coordenadora já ter pedido pra fazer direto.
- Alterar ou fechar task de outra pessoa passa por confirmação, mesmo com autorização prévia de gravação.
- Nunca diga que anexou um arquivo, só que incluiu o link na descrição (o Taskflow, por essa integração, não tem ferramenta de anexar arquivo).
- Nunca peça, mostre ou solicite um ID técnico (de projeto, tarefa, pessoa ou workspace) pra coordenadora — são só de uso interno desta skill.
- Antes de assumir os projetos/pessoas fixos documentados aqui, confira se ainda batem (`Taskflow:list_projects`); se a coordenadora mencionar uma área ou pessoa nova, verifique antes de assumir que precisa criar algo do zero.
- Se a coordenadora disser que o status aparece como "Backlog" na tela, é sinal de que o valor gravado não bate exatamente com uma opção do seletor: peça um print da lista de opções e corrija com o valor exato.
- **Se uma pessoa nova entrar no time:** peça pra coordenadora adicioná-la a um projeto do Taskflow (ou confirme que já foi adicionada); compare a lista de membros desse projeto antes e depois com `Taskflow:list_projects` pra achar o ID novo por eliminação; crie uma task de teste nesse projeto atribuída a esse ID e confira o `assigneeName` devolvido pra confirmar quem é; só depois disso, proponha atualizar esta skill com o ID confirmado. Apague a task de teste, ou peça pra coordenadora apagar depois (esta integração não tem ferramenta de exclusão).
- Se o modelo de dados do Taskflow mudar de novo (por exemplo, ganhar dependência real, seções ou status automático a partir das subtasks), esta é a skill a reescrever primeiro — a `exitus-organizar-tarefa` não muda, ela nunca dependeu de qual ferramenta opera o Taskflow.