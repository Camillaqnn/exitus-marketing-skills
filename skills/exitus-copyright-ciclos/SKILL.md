---
name: "exitus-copyright-ciclos"
description: Escreve, em etapas (primeiro topo, depois meio, depois fundo de funil, cada uma só após a aprovação da anterior), as sugestões de direcionamento (descrição visual + copy da peça) e de legenda das pautas semanais de ciclo de postagem do Exitus, desdobradas em Feed Instagram, Story, LinkedIn e canal do Telegram, usando o banco de copys aprovadas como referência. Cuida só do texto; registro e pauta ficam com exitus-bancodecopys-ciclos e exitus-pautar-ciclos. Use SEMPRE que qualquer pessoa do time mandar a foto do quadro com a pauta da semana, pedir para usar a gravação/transcrição da reunião de estratégia ou de sprint no Fireflies, mandar os conteúdos decididos em texto, falar em pauta, ciclo, cíclo de postagens, conteúdo semanal, conteúdo das redes do Exitus, post de Instagram, LinkedIn, story ou Telegram, legenda, direcionamento, copy de peça, carrossel, story ou canal, pedir para refinar ou reescrever uma peça, ou disser que uma etapa ou o ciclo foi aprovado, mesmo sem citar a skill pelo nome.
compatibility: Entrada por reunião requer o conector Fireflies (fireflies_get_transcripts, fireflies_get_summary, fireflies_get_transcript). Leitura do banco de copys requer os conectores Google Drive (search_files, get_file_metadata) e Google Docs (read_doc).
---

# Copy dos ciclos de postagem do Exitus

> Versão 2.3 (19/11/2026) — histórico de mudanças em `CHANGELOG.md`.

Qualquer pessoa do time pode pedir. Nesta skill, "a pessoa" é quem fez o pedido.

Toda segunda-feira o marketing decide os 3 conteúdos da semana (topo, meio e fundo de funil). A pessoa manda esses conteúdos e esta skill devolve **sugestões** de direcionamento e legenda para cada peça. Ela vai refinando peça por peça depois, então o objetivo é um primeiro rascunho forte e fácil de ajustar, não um texto final engessado.

Antes de escrever, carregue também a skill **exitus-voz-e-marca** (tom de voz, vocabulário, nomes de produto atuais, fórmula de IA com validação humana). Esta skill cuida da estrutura do ciclo e do formato; aquela cuida da voz.

Esta skill cuida **só do material escrito**. Registrar as copys aprovadas é trabalho da skill `exitus-bancodecopys-ciclos`, e gravar a pauta no **Calendário de conteúdo** (o artefato onde a coordenadora e o designer se organizam; o Google Slides não é mais usado) é da `exitus-pautar-ciclos` (seção 8).

A referência principal de como as copys devem ser feitas é o **banco de copys aprovadas** no Google Docs (seção 2.1). O arquivo `references/banco-referencias-ciclos.md` guarda os ciclos de setembro até 15/11/2026: leia sempre a seção 7 dele (ganchos, enquetes, estruturas e cenas já usados) e use o resto como apoio quando o banco não tiver o ciclo do mesmo produto.

## 1. Como o ciclo é montado

São **3 conteúdos**, um por etapa do funil, e cada um vira **4 peças**:

| Etapa | Papel na semana | Fecha com |
|---|---|---|
| **Topo** | Mostra a dor como ela é vivida no dia a dia (pilha de redações, WhatsApp lotado, mesa bagunçada). Não cita produto. | Pergunta que faz o público se reconhecer |
| **Meio** | Mostra que existe outro caminho: a IA resolvendo a dor de forma genérica, ou uma isca de valor (ex.: 5 prompts). Ainda sem vender. | Pergunta de imaginação ("Já imaginou…?") ou CTA de isca ("Comente 'PALAVRA'") |
| **Fundo** | Apresenta o produto Exitus que resolve aquela dor, com a IA apoiando e a pessoa conduzindo. | CTA de conversa com o time comercial |

Os três conteúdos contam **uma história contínua**: o meio retoma a cena do topo, o fundo retoma o meio ("Lembra daquela pilha de material?"). Mantenha os mesmos elementos de cena e as mesmas palavras-chave ao longo da semana.

| Peça | Formato | O que gerar |
|---|---|---|
| **Feed Instagram** | 1080x1350 (estático ou carrossel) | Direcionamento + legenda |
| **Story** | 1080x1920 | Só direcionamento (não tem legenda) |
| **LinkedIn** | Mesma arte do feed | Só legenda |
| **Canal do Telegram** | 1080x1080, adaptação da arte do feed | Direcionamento da adaptação + legenda |

## 2. Entrada: foto do quadro, gravação da reunião ou texto

A pauta da semana chega de uma de três formas. Trate todas igual depois de transcrever.

**Foto do quadro (manuscrito).** Toda segunda o time escreve a semana num quadro branco e a pessoa manda a foto. Leia assim:

- Cada linha começa com dia e data ("Seg 16", "Qua 18").
- Depois vem o tema, e no fim os formatos daquele dia ("Feed / LinkedIn / Canal" ou "Story").
- Linhas com "↳" embaixo de um tema são **ângulos/subtemas** que o conteúdo deve abordar.
- Aspas ou duas barras ("〃", "||", "11") no lugar do tema significam **mesmo tema da linha anterior**, normalmente o story que acompanha o feed do dia antes.
- A ordem da semana define o funil: o primeiro tema é topo, o segundo é meio, o terceiro (geralmente com nome de produto) é fundo.
- "Canal" é o canal do Telegram.

Comece a resposta com a transcrição do quadro numa tabela curta (dia, tema, ângulos, formatos, funil). Se alguma palavra estiver ilegível, escreva o que você entendeu com "(?)" e siga em frente; a pessoa corrige se precisar.

**Gravação da reunião de estratégia (Fireflies).** Toda segunda há a reunião de Montagem de Sprint do marketing/comercial e, logo depois, a reunião da pessoa com a equipe de marketing, onde se decidem abordagens, datas e estratégias de postagem da semana. Essa segunda reunião é a que interessa. Quando a pessoa pedir para usar a reunião ou a gravação (ou disser que a pauta saiu da reunião), faça assim:

1. **Encontre a reunião** com `fireflies_get_transcripts` (`mine: true`, `fromDate` e `toDate` no dia da reunião, normalmente a segunda da semana do ciclo). As gravações costumam ter título genérico de data e hora ("Nov 09, 11:00 AM"); a Montagem de Sprint às vezes vem nomeada ("Seg, Oct 26, 09:06 - Montagem de Sprint"). A reunião de estratégia de conteúdo é a que vem **depois** da Montagem de Sprint no mesmo dia.
2. **Confirme pelo conteúdo, não só pelo horário.** Leia o resumo (`fireflies_get_summary`) e procure sinais de pauta de conteúdo: topo/meio/fundo, feed, story, LinkedIn, canal/Telegram, datas da semana, temas de post. Se houver mais de uma candidata, ou nenhuma bater, liste as opções (título, horário, duração e uma linha do resumo) e pergunte qual usar. Se a reunião do dia ainda não tiver transcrição (sem falas, resumo "skipped" ou ainda processando), avise e peça para tentar mais tarde ou mandar a foto do quadro.
3. **Leia a transcrição completa** com `fireflies_get_transcript`. O resumo serve para achar a reunião; a pauta sai das falas, porque o resumo costuma perder ângulos e detalhes.
4. **Extraia o que foi decidido**, separando decisão de ideia descartada ou "talvez": os 3 conteúdos e o funil de cada um, os ângulos/subtemas, datas e dias de cada formato, horários, produto, referências citadas (posts, concorrentes, peças anteriores), pedidos de tom ou abordagem ("mais leve", "sem citar preço", "usar a professora") e qualquer regra pontual da semana. Quando algo foi discutido sem fechamento, marque como "(em aberto)" em vez de decidir sozinho.
5. **Mostre a transcrição da pauta** na mesma tabela da foto do quadro (dia, tema, ângulos, formatos, funil), com uma coluna "Na reunião" trazendo o link do momento em que aquilo foi decidido: `https://app.fireflies.ai/view/{id}?t={segundos}`. Abaixo da tabela, liste em poucas linhas as orientações de abordagem e os pontos em aberto. Siga para o topo normalmente; a pessoa corrige a tabela se algo veio errado.
6. As orientações de abordagem tiradas da reunião valem para a semana inteira: aplique-as também no meio e no fundo.

Se a pessoa mandar a foto do quadro **e** a reunião, a foto define a estrutura (dias, temas, formatos) e a reunião complementa com ângulos e orientações. Se as duas se contradisserem, pergunte antes de seguir.

**Texto.** Da mensagem, extraia para cada conteúdo: tema/nome, etapa do funil, produto (se houver), formatos, data e horário.

Em qualquer das três, o que faltar (estático ou carrossel, horário) você assume de forma sensata e sinaliza numa linha no fim ("Assumi carrossel no topo; se for estático me avisa"). Não trave a entrega com perguntas.

## 2.1. Antes de escrever: carregue o que o time já aprendeu

O banco de copys é a referência principal. Ele guarda só o que o time aprovou, então vale mais que qualquer exemplo de `references/`.

1. **Leia o banco do mês.** Fica no drive compartilhado **Criação Marketing Exitus**, em `Arquivos > Banco de copys > AAAA > NN - Mês`, num Google Doc chamado `[EXITUS] Banco de Copys de Ciclos - Mês` (um por mês). IDs conhecidos: pasta `Banco de copys` `1iUnWQUQtLICQV7sL7cjqEwHXVtjBCP9j`; pasta `2026` `14_ir2eKi-2ccsNla1OAP9msGmkEY_puB`; doc de novembro de 2026 `1J5lfZeZrrSQmB61k8kEDP0Bvf9rK4kf4BUYtTovGTyM`. Para os outros meses, ache a pasta do ano dentro de `Banco de copys` com `search_files`, liste as pastas de mês dentro dela e abra o doc da pasta do mês.
2. **Comece pela seção "Aprendizados"**, no topo do doc. São as regras que se repetem entre ciclos. Aprendizado que contradiz uma regra desta skill vale mais que a regra: siga o aprendizado.
3. **Leia os ciclos aprovados do mês atual e do mês anterior**, e procure ciclos do mesmo produto ou dor em meses mais antigos. Use para calibrar tom, tamanho e estrutura, e para **não repetir** ganchos, cenas e enquetes já usados.
4. Se precisar ver como uma peça ficou na pauta, a pauta de cada ciclo fica no **Calendário de conteúdo** (`https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC`): cada postagem tem o direcionamento, o texto da peça, a legenda, a arte enviada pelo designer e a conversa sobre ela (no banco do artefato, coleção `posts`). As pautas antigas no Google Slides (`Criação Marketing Exitus > AAAA > Cíclo de Postagens`, até o ciclo de 23/11 a 05/12/2026) foram migradas para o calendário e ficam só como consulta: leia, mas nunca crie nem edite slides ou comentários.
5. Se não encontrar o banco ou o Drive, siga com `references/banco-referencias-ciclos.md` e avise em uma linha que o banco não foi lido.

## 2.2. Confira o calendário da semana

Antes de entregar o topo, cheque feriados e datas comemorativas de todos os dias do ciclo. Use `references/datas-calendario.md` como lista de apoio e **confirme sempre com busca na web** para o ano em questão (ex.: "feriados novembro 2026", "feriado Ceará novembro 2026", "feriado Fortaleza 2026", "datas comemorativas educação novembro", "datas ENEM 2026"). Considere feriados nacionais, do Ceará e de Fortaleza.

Procure duas coisas:

1. **Feriado no dia de alguma peça.** Sinalize e ofereça opções, sem decidir sozinho: mover a peça para outro dia da semana, manter com o tom ajustado, ou trocar por um post da data. Atenção especial ao **fundo de funil**: CTA de "agende uma conversa" num feriado cai num dia sem atendimento comercial, então sugira mover para antes do feriado. Para datas de luto ou memória, não sugira manter peça comercial no dia.
2. **Data do âmbito educacional na semana** (Dia do Professor, Dia do Diretor, Dia do Estudante, provas e resultados do ENEM, volta às aulas etc.). Sugira uma peça avulsa, normalmente um story de homenagem ou de apoio e às vezes um feed, fora do funil e sem CTA comercial, no padrão das peças de data já publicadas.

Mostre o resultado na primeira resposta, logo abaixo da tabela da pauta, num bloco curto **"Calendário da semana"**: a data, o que é, o impacto no ciclo e a sugestão. Se não houver nada relevante, diga isso em uma linha.

As peças de data são **sugestões até a pessoa aprovar a ideia**. Se ela aprovar, escreva a peça junto com a etapa do funil cuja data está mais próxima e trate-a como parte daquela etapa na aprovação. Se a pessoa mover uma peça por causa de feriado, use a nova data em todas as etapas seguintes.

## 3. Formato da saída

**O ciclo é entregue por etapas, um conteúdo por vez.** Nunca entregue os três conteúdos de uma vez.

1. **Primeira resposta:** a transcrição da pauta (tabela com os 3 conteúdos, para a pessoa conferir a semana inteira) e **só o conteúdo de topo**, com as 4 peças.
2. A pessoa refina o topo peça por peça (seção 7) até dizer que **o topo está aprovado**.
3. Só então entregue o **meio**, já apoiado na versão aprovada do topo, para a história da semana continuar dela: mesma cena, mesmas palavras-chave, "Lembra…?" no Telegram.
4. Refinamento do meio até a aprovação, e então o **fundo**, apoiado no topo e no meio aprovados.
5. Quando o fundo for aprovado, o ciclo está fechado. Veja a seção 8 para o que acontece depois de cada aprovação.

Uma aprovação libera a etapa seguinte. Mensagens como "aprovado", "pode seguir", "fechou o topo" ou "vamos pro meio" contam como aprovação da etapa atual. Ao entregar uma etapa nova, não repita as etapas já aprovadas.

Termine cada entrega com uma linha dizendo em que etapa o ciclo está e o que vem depois, por exemplo: "Topo (1/3). Quando aprovar, sigo para o meio."

Dentro de cada conteúdo, organize as peças na ordem Feed, Story, LinkedIn, Telegram. Comece cada peça com o cabeçalho padrão, o mesmo que o banco de copys usa:

`Canal — Nome do conteúdo | Formato | Funil | Horário — DD.MM`

Ex.: `Feed Instagram — Dor da matrícula | Carrossel | Topo | 12h — 16.11`, `Story — Dor da matrícula | Enquete | Topo | 12h — 17.11`, `LinkedIn — Dor da matrícula | Carrossel | Topo | 12h — 16.11`, `Telegram — Dor da matrícula | Estático | Topo | 12h — 16.11`. No story, o "Formato" é a interação (Enquete, Quiz ou Link de agendamento).

Use os rótulos que o time já usa nas pautas (a skill exitus-pautar-ciclos grava cada parte num campo da postagem do calendário: DIRECIONAMENTO vira "Direcionamento", cada TELA ou bloco da COPY vira uma linha de "Texto da peça" e LEGENDA vira "Legenda"):

```
DIRECIONAMENTO:
Cena: ...
Enquadramento: ...

COPY:
TELA 1
Título: ...
Texto corrido: ...
Destaque: LINHA FINAL EM CAIXA ALTA

TELA 2
...

LEGENDA:
...
```

Peça estática não usa "TELA"; liste os itens rotulados direto.

**O direcionamento não repete formato nem tamanho.** O cabeçalho já diz se é Carrossel, Estático, Story ou Canal, e cada formato tem tamanho fixo. Comece direto pelo visual: nada de "Carrossel de 3 telas, 1080x1350.", "1 tela, 1080x1920." ou "Adaptação 1080x1080".

**Toda linha de copy da peça leva a categoria na frente**, para o designer enxergar a hierarquia. Use só estes rótulos: `Título:` (frase principal), `Subtítulo:` (frase curta logo abaixo do título), `Texto corrido:` (frase ou parágrafo de apoio mais longo), `Lista:` (itens, um por linha abaixo do rótulo), `Etiquetas:` (pills separadas por " · "), `Destaque:` (linha em caixa alta que não é chamada para ação), `CTA:` (chamada para ação; posição entre parênteses se importar, ex.: `CTA (rodapé):`), `Enquete:` / `Quiz:` (interação do story, com as opções) e `Sticker de link:`. Não use "Apoio:" nem linhas soltas sem rótulo. **Categoria vazia não aparece**: se a peça não tem subtítulo, não escreva "Subtítulo: —", só pule a linha.

**O direcionamento também é rotulado**, um item por linha: `Cena:` e `Enquadramento:` para foto, `Layout:` para peça tipográfica ou sem foto, `Mockup:` para tela do produto ou conversa, `Tela N:` em cada tela do carrossel, `Adaptação:` no Telegram e `Referência:` para links. Use só os que se aplicam.

## 4. Regras do direcionamento (para o designer)

**Sempre inclua o bloco visual.** Nas pautas antigas ele às vezes faltava e o designer precisava adivinhar. Diga se é foto, mockup (WhatsApp, tela do produto), layout tipográfico ou combinação. Quando for foto, use o padrão do time:

- **Cena:** quem está, o que está fazendo, objetos que contam a história.
- **Enquadramento:** close, plano aberto, top-down; o que fica em foco e o que fica desfocado.
- Mantenha continuidade entre telas (mesma pessoa, mesma roupa) e entre peças da semana.

Use pessoas reais em ambientes reais de escola, conforme a direção de fotografia da marca. Quando fizer sentido, sugira personagens que a marca já usa nas peças.

**Não use a Tia Cris nem o Tio Bento.** Eles foram criados pelo Colégio Christus e saíram da marca com a emancipação do Exitus; os novos tutores virtuais ainda estão em criação. Se a peça precisar de um tutor, escreva "novo tutor virtual (a definir)" no direcionamento, sem nome nem aparência.

**A copy da peça é muito mais curta que a legenda.** A peça dá o golpe, a legenda desenvolve. Na peça:

- Título curto e forte; subtítulo ou texto corrido de 1 a 2 frases.
- Fechamento em CAIXA ALTA, rotulado como `Destaque:` (punchline, pergunta) ou `CTA:`.
- Listas e etiquetas são bem-vindas: etiquetas/pills no topo separadas por " · ", listas de tarefas (que podem aparecer riscadas, "como lista de supermercado"), cronogramas tipo "SEG · Tema".
- Carrossel com **no máximo 3 telas**, e a **última tela sempre traz o CTA** (no topo e no meio, a pergunta ou convite de fechamento; no fundo, o CTA comercial).
- Na semana, **pelo menos um dos 3 posts de feed é de tela única** (estático).
- Pode indicar recursos como mockup de conversa ("[chat] Aluno: … / Tutor: …"), ícone de "arraste para o lado", ou "OBS:" com observações de diagramação.

**Story** é interação rápida e tem **obrigatoriamente 1 tela só**:

- Topo e meio: **título + interação** (pergunta + enquete ou quiz com 2 a 4 opções curtas e reconhecíveis, de preferência uma bem-humorada, como "Nem me pergunte 😅"). **Subtítulo só se o contexto pedir**; se não for necessário, não use. **Sem CTA.**
- Fundo: é o **único story com CTA**. Produto em destaque (tela do produto ocupando boa parte do story), frase curta e "Sticker de link: AGENDAR CONVERSA".

**Telegram** é a arte do feed adaptada para quadrado, com **menos texto**: fique com o título e no máximo uma frase de apoio. Se a adaptação for direta, escreva "Adaptação do feed" e diga o que entra e o que sai, sem citar o tamanho.

## 5. Regras das legendas

### Feed Instagram
- Abre com gancho curto (cena, número, pergunta) e emoji **no fim da primeira linha**.
- 3 a 5 linhas curtas, falando direto com professor/equipe: "você", "seus alunos", "sua turma".
- Topo e meio fecham com pergunta. Fundo fecha com "Agende uma conversa pelo link na bio e conheça o Exitus X!" ou "Comente 'PALAVRA' que o nosso time fala com você".
- 5 hashtags no fim, sempre em Title Case com acento: `#ENEM2026 #Educação #Professores #GestãoEscolar #Exitus`. A última é sempre #Exitus.

### LinkedIn
- Mesmo assunto, ângulo de **gestão**: fala com a instituição ("a sua instituição", "a equipe"), não com o aluno.
- Sem emoji. Parágrafos mais analíticos: o custo invisível da dor, o processo, o que isso pesa no resultado.
- Pode ser mais longo que o Instagram (4 a 7 parágrafos curtos).
- Quando houver IA, deixe o papel humano explícito (ex.: "A IA organiza. O professor acompanha e conduz.").
- Pergunta final que abre debate sobre a realidade da instituição. No fundo: "Agende uma conversa com o nosso time e saiba mais sobre o Exitus X."
- Se o CTA depender do Instagram (comentar palavra-chave), direcione: "comente 'PALAVRA' no post fixado do nosso Instagram @exituseduc".
- 5 hashtags em Title Case com acento, mais institucionais (#GestãoEscolar #InteligênciaArtificial #InstituiçõesDeEnsino).

### Canal do Telegram
- A mais curta: 2 a 3 linhas, tom de conversa ("aí", "Sabe aquelas…?"), um emoji.
- Continuidade explícita entre os 3 posts da semana ("Lembra dos 5 prompts?").
- **As pessoas não conseguem responder no canal.** Nunca use "responde aqui", "responda X" ou "comente". Use CTA de reação ("Reaja a este conteúdo que a gente te envia o material") ou de agendamento ("Agende sua conversa e saiba mais").
- Sem hashtags.

## 6. Antes de entregar, confira

- [ ] Entreguei só a etapa atual, e ela continua a história das etapas já aprovadas?
- [ ] Topo não cita produto; fundo cita o produto com o nome atual?
- [ ] Toda peça de feed, story e Telegram tem bloco visual no direcionamento, sem repetir formato ou tamanho?
- [ ] Toda linha de copy da peça tem rótulo de categoria (Título, Subtítulo, Texto corrido, Lista, Destaque, CTA…)?
- [ ] Carrosséis com até 3 telas, CTA na última, e pelo menos um post de tela única na semana?
- [ ] Stories com 1 tela só, e CTA apenas no story do fundo?
- [ ] Li os Aprendizados do banco de copys e não repeti, sem intenção, gancho, enquete ou cena já usados (banco + seção 7 de `references/banco-referencias-ciclos.md`)?
- [ ] Nenhuma peça usa a Tia Cris ou o Tio Bento?
- [ ] A copy da peça está bem mais curta que a legenda?
- [ ] Legendas realmente diferentes por canal (não é a mesma com cortes)?
- [ ] Nenhum CTA de resposta/comentário no Telegram?
- [ ] Hashtags em Title Case com acento, #Exitus por último no Instagram?
- [ ] Datas e contagens ("faltam X dias") batem com a data de publicação?
- [ ] Chequei feriados (nacionais, Ceará e Fortaleza) e datas educacionais da semana, e nenhuma peça com CTA comercial caiu em feriado sem aviso?
- [ ] Menção a IA deixa claro que a pessoa revisa, conduz ou decide?

## 7. Refinamento

Quando a pessoa pedir ajuste numa peça, devolva **só aquela peça** (ou só o bloco alterado), não a etapa inteira. Se o ajuste mudar algo que as outras peças da mesma etapa repetem (ex.: o gancho do feed que o Telegram reaproveita), avise em uma linha quais peças podem precisar acompanhar.

Mantenha sempre a **versão aprovada de cada etapa** como base. Se, já no meio ou no fundo, a pessoa pedir para mudar algo de uma etapa aprovada, ajuste aquela peça, diga em uma linha o que nas etapas seguintes pode precisar acompanhar, e trate a peça ajustada como a nova versão aprovada.

## 8. Depois de cada aprovação

Esta skill não grava nada: ela entrega o material escrito e passa a vez.

1. **A cada etapa aprovada** (topo, meio ou fundo, e peças de data aprovadas junto com elas), carregue a skill **exitus-bancodecopys-ciclos** e registre aquela etapa no banco do mês, com a versão final aprovada e a sua sugestão original desta conversa para comparação. Depois volte e entregue a próxima etapa.
2. **Quando o fundo for aprovado**, depois do registro, carregue a skill **exitus-pautar-ciclos** e passe as versões finais aprovadas de topo, meio e fundo (não as sugestões originais) para ela gravar a pauta no Calendário de conteúdo, onde o designer executa as peças. Nunca monte pauta no Google Slides.
3. **Ajuste depois de aprovado:** se a pessoa mudar uma peça já registrada, entregue a peça nova aqui e acione a `exitus-bancodecopys-ciclos` para atualizar o registro e, se a pauta já estiver no calendário, a `exitus-pautar-ciclos` para trocar só aquela postagem.
