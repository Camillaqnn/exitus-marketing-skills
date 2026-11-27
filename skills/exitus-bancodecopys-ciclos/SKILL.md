---
name: "exitus-bancodecopys-ciclos"
description: Registra num Google Docs, a cada aprovação, as copys aprovadas dos ciclos de postagem do Exitus (direcionamento, copy da peça e legendas de Feed Instagram, Story, LinkedIn e Telegram), no banco de copys do mês (pasta Arquivos/Banco de copys/AAAA/NN - Mês do drive Criação Marketing Exitus), e mantém a seção "Aprendizados" que a skill exitus-copyright-ciclos usa como referência de como as copys devem ser feitas. Use SEMPRE que uma etapa (topo, meio ou fundo) ou peça de um ciclo for aprovada, quando uma copy já aprovada for ajustada, ou quando qualquer pessoa do time pedir para salvar, registrar, guardar, arquivar, atualizar ou consultar copys aprovadas, o banco de copys ou os aprendizados dos ciclos, mesmo sem citar a skill pelo nome.
compatibility: Requer os conectores Google Drive (search_files, create_file, get_file_metadata, update_file) e Google Docs (read_doc, update_doc).
---

# Exitus - Banco de Copys dos Ciclos

> Versão 1.3 (19/11/2026) — histórico de mudanças em `CHANGELOG.md`.

Qualquer pessoa do time pode pedir. Nesta skill, "a pessoa" é quem fez o pedido ou aprovou a etapa.

Esta skill guarda, num Google Doc por mês, **tudo o que o time aprovou** nos ciclos de postagem. O banco tem duas funções: ser o registro oficial das copys finais e ser a referência que a skill `exitus-copyright-ciclos` lê antes de escrever um ciclo novo. Por isso ele guarda só o que foi aprovado, exatamente como foi aprovado.

Ela cuida **só do registro**: não escreve copy nova (isso é da `exitus-copyright-ciclos`) e não mexe na pauta, que fica no **Calendário de conteúdo** (isso é da `exitus-pautar-ciclos`; o Google Slides não é mais usado). Nunca registre nada no Notion.

## 1. Onde fica

```
Criação Marketing Exitus > Arquivos > Banco de copys > AAAA > NN - Mês > [EXITUS] Banco de Copys de Ciclos - Mês
```

Exemplos: o banco de novembro de 2026 fica em `Banco de copys > 2026 > 11 - Novembro`; o de dezembro, em `Banco de copys > 2026 > 12 - Dezembro`; o de janeiro de 2027, em `Banco de copys > 2027 > 01 - Janeiro`. O ano e o mês seguem o primeiro dia da semana do ciclo.

IDs conhecidos (atalhos; se um não bater, ache pelo nome):
- pasta `Arquivos`: `1_uDrRiZ9Cbgoyu2P2rJ3kblGKJq1fQnN`
- pasta `Banco de copys`: `1iUnWQUQtLICQV7sL7cjqEwHXVtjBCP9j`
- pasta `Banco de copys > 2026`: `14_ir2eKi-2ccsNla1OAP9msGmkEY_puB`
- pasta `Banco de copys > 2026 > 11 - Novembro`: `1jRzfjQK2jNakwHSSq0UFwxnZ751-Ea0C`
- doc `[EXITUS] Banco de Copys de Ciclos - Novembro`: `1J5lfZeZrrSQmB61k8kEDP0Bvf9rK4kf4BUYtTovGTyM`

É **um documento por mês**. O ano e o mês são os do **primeiro dia da semana** do ciclo. Um ciclo que cruza dois meses fica inteiro no doc do primeiro mês.

Se encontrar uma pasta de mês solta direto em `Banco de copys` (fora da pasta do ano), mova-a para dentro da pasta do ano certo com `update_file` antes de registrar, e avise em uma linha.

O doc antigo `Banco de Copys – Ciclos de Postagem` (solto na pasta `Banco de copys`) foi substituído pelos docs mensais. Não registre nada nele.

## 2. Encontrar ou criar a pasta e o documento

Antes de criar ou editar qualquer arquivo, carregue a skill **google-workspace**.

1. **Pasta do ano.** Procure dentro de `Banco de copys` com `search_files` (`parentId = '1iUnWQUQtLICQV7sL7cjqEwHXVtjBCP9j' and title = 'AAAA' and mimeType = 'application/vnd.google-apps.folder'`). Se não existir (por exemplo, no primeiro ciclo de 2027), crie com `create_file` (`title`: `AAAA`, `contentMimeType`: `application/vnd.google-apps.folder`, `parentId`: ID de `Banco de copys`).
2. **Pasta do mês.** Liste as subpastas da pasta do ano e pegue a do mês (`NN - Mês`, dois dígitos e mês por extenso com inicial maiúscula: `12 - Dezembro`, `01 - Janeiro`). Se não existir, crie do mesmo jeito, com `parentId` = ID da pasta do ano. Antes de criar qualquer pasta, confira se não há uma com nome parecido, para não duplicar.
3. **Doc do mês.** Procure dentro da pasta do mês o Google Doc `[EXITUS] Banco de Copys de Ciclos - Mês` (ex.: `… - Dezembro`). Se não existir, crie com `create_file` (`title` nesse padrão, `contentMimeType`: `application/vnd.google-apps.document`, `parentId`: ID da pasta do mês) e monte a estrutura inicial da seção 3, **copiando a seção "Aprendizados" do doc do mês anterior** (na virada do ano, o doc de dezembro do ano anterior), para os aprendizados não se perderem.
4. Diga em uma linha, na resposta, se criou ou moveu pasta ou doc.

## 3. Estrutura do documento

```
[EXITUS] Banco de Copys de Ciclos - Novembro                 (Título)
Copys finais aprovadas pelo time nos ciclos de postagem deste mês, do ciclo mais recente para o mais antigo. Só entra o que foi aprovado.

Aprendizados                                                  (Título 1)
• Regra geral que se repete entre ciclos.

Ciclo 16 a 21/11 – Exitus Matrícula                           (Título 1)
Pauta: Calendário de conteúdo, ciclo 16 a 21/11 (link do calendário, quando a pauta for gravada)
Fio condutor: uma ou duas frases sobre a história da semana.
Datas: topo seg 16.11 12h · meio qua 18.11 12h · fundo qui 19.11 12h
Status: Topo aprovado em 16/11 · Meio aprovado em 17/11 · Fundo aprovado em 18/11

Topo – Rotina do comercial                                    (Título 2)
Feed Instagram — Rotina do comercial | Estático | Topo | 12h — 16.11   (Título 3)
Direcionamento:
Cena: …
Enquadramento: …
Copy da peça:
Título: …
Texto corrido: …
Destaque (rodapé): …
Legenda:
…
LinkedIn — Rotina do comercial | Estático | Topo | 12h — 16.11       (Título 3)
Legenda:
…
Story — Rotina do comercial | Enquete | Topo | 12h — 17.11           (Título 3)
Direcionamento: …
Copy da peça: …
Telegram — Rotina do comercial | Estático | Topo | 12h — 16.11       (Título 3)
Direcionamento:
Adaptação: …
Copy da peça: …
Legenda:
…
O que mudou da sugestão para a versão final                  (Título 3)
• Frase curta por mudança.

Meio – …                                                      (Título 2)
Fundo – …                                                     (Título 2)
Peças de data (se houver)                                     (Título 2)
```

Regras de formatação:
- Cabeçalho de cada peça no padrão da `exitus-copyright-ciclos`: `Canal — Nome do conteúdo | Formato | Funil | Horário — DD.MM`. No story, o formato é a interação (Enquete, Quiz ou Link de agendamento).
- "Direcionamento:", "Copy da peça:" e "Legenda:" em negrito, cada um numa linha, com o conteúdo abaixo.
- Dentro do direcionamento e da copy, use **os mesmos rótulos** da `exitus-copyright-ciclos` (`Cena:`, `Enquadramento:`, `Layout:`, `Mockup:`, `Tela N:`, `Adaptação:`, `Referência:`, `Título:`, `Subtítulo:`, `Texto corrido:`, `Lista:`, `Etiquetas:`, `Destaque:`, `CTA:`, `Enquete:`, `Quiz:`, `Sticker de link:`), rótulo em negrito e conteúdo em peso normal. Carrossel usa `TELA N` antes dos itens de cada tela.
- Legendas copiadas **exatamente** como aprovadas: quebras de linha, emojis e hashtags.
- Ciclo mais recente logo abaixo de "Aprendizados". Dentro do ciclo, a ordem é sempre Topo, Meio, Fundo, Peças de data.

## 4. Quando e o que registrar

**A cada etapa aprovada.** Mensagens como "aprovado", "pode seguir", "fechou o topo" ou "vamos pro meio" contam como aprovação.

**Antes de registrar, confira se a etapa já está no doc.** Esta skill pode ser acionada duas vezes pela mesma aprovação (pela própria descrição e pela `exitus-copyright-ciclos`). Se a entrada do ciclo já tem aquela etapa, não registre de novo: compare com a versão aprovada e, se houver diferença, trate como ajuste (abaixo); se não houver, só confirme que já estava registrada.

1. **Topo aprovado:** crie a entrada do ciclo (Título 1 e as linhas de Pauta, Fio condutor, Datas e Status) e registre o topo com as 4 peças e o bloco "O que mudou".
2. **Meio e fundo aprovados:** registre a etapa dentro da entrada do mesmo ciclo, na ordem, e atualize a linha de Status. Não crie uma entrada nova.
3. **Peças de data aprovadas** entram na seção "Peças de data" do ciclo.
4. **Ciclo fechado (fundo registrado):** atualize os Aprendizados (seção 5).
5. **Pauta gravada:** a `exitus-pautar-ciclos` aciona esta skill ao terminar. Troque o texto da linha "Pauta:" por `Calendário de conteúdo, ciclo DD a DD/MM` com o link do calendário (`https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC`).

**O que mudou.** Compare a versão aprovada com a sugestão original da `exitus-copyright-ciclos` nesta conversa e escreva em frases curtas o que mudou e por quê (o porquê vem do padrão da mudança: ficou mais curto, trocou o gancho, tirou jargão, mudou o CTA). Se a sugestão original não estiver disponível, escreva "Sugestão original não disponível nesta conversa."

**Ajuste depois de registrado.** Troque o texto daquela peça no lugar, sem duplicar a entrada, e acrescente no "O que mudou" da etapa uma linha `Ajustado em DD/MM: …`.

**Versões finais vindas de fora.** Se alguém colar as versões finais no chat ou apontar a pauta no calendário, registre a partir delas. No calendário, cada postagem tem os campos "Direcionamento", "Texto da peça" e "Legenda" (no banco do artefato, coleção `posts`). Pautas antigas no Google Slides (até o ciclo de 23/11 a 05/12/2026) trazem o direcionamento nos quadros "DESCRIÇÃO DA IMAGEM / TEXTO DA IMAGEM" e a legenda na caixa "Legenda:".

**Nunca registre** sugestão que não foi aprovada, e nunca altere o registro de outro ciclo.

**Fornecer o ciclo para a pauta.** Se a `exitus-pautar-ciclos` precisar das versões aprovadas e elas não estiverem na conversa, leia a entrada do ciclo no doc do mês e devolva, por etapa, o cabeçalho, o direcionamento, a copy da peça e as legendas de cada peça, exatamente como estão registrados.

## 5. Aprendizados

A seção "Aprendizados" é o que a `exitus-copyright-ciclos` lê primeiro antes de escrever. Ela traz regras gerais que se repetem entre ciclos, não detalhes de uma peça.

- Ao fechar um ciclo, releia os blocos "O que mudou" das três etapas e veja se algum padrão se repete neste ciclo ou em ciclos anteriores (ex.: "o time sempre corta a segunda frase da legenda do Telegram").
- Também entram decisões que valem dali em diante, mesmo vindas de uma única vez, quando a pessoa disser que é regra (ex.: assinatura oficial de um produto, hashtag de campanha).
- Mantenha a seção curta: consolide e reescreva em vez de só acrescentar, e remova o que deixou de valer. Cada item numa linha.
- Na virada do mês, a seção é copiada para o doc novo (seção 2).

## 6. Como escrever no Docs

1. Leia o doc com `read_doc` logo antes de escrever, para pegar a posição certa e não sobrescrever o que outra pessoa acabou de mudar.
2. Insira o texto no ponto certo (entrada nova logo abaixo de "Aprendizados"; etapa nova no fim da entrada do ciclo) com `update_doc`, seguindo a skill **google-workspace** para índices e estilos.
3. Aplique os estilos de parágrafo (Título 1, 2 e 3) e o negrito dos rótulos.
4. Releia o trecho escrito e confira se não sobrou marcador, linha duplicada ou peça faltando.

## 7. Confirmar

Responda em uma linha o que foi registrado e onde, com o link do doc (ex.: "Topo do ciclo 16 a 21/11 registrado no banco de novembro."). Depois, o fluxo continua: a `exitus-copyright-ciclos` entrega a próxima etapa ou, se o fundo foi aprovado, a `exitus-pautar-ciclos` grava a pauta no Calendário de conteúdo.
