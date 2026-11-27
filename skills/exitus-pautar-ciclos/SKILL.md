---
name: "exitus-pautar-ciclos"
description: Grava a pauta semanal do ciclo de postagens do Exitus direto no Calendário de conteúdo (artefato https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC) — cria o ciclo e uma postagem por peça (feed, story, LinkedIn e canal do Telegram) com data, horário, formato, etapa, direcionamento, texto da peça e legenda, para o designer executar. O Google Slides não é mais usado. Cuida só da pauta; a copy vem da exitus-copyright-ciclos e o registro no banco é da exitus-bancodecopys-ciclos. Use SEMPRE que o fundo de um ciclo for aprovado, ou quando pedirem para montar, gerar, subir, passar, atualizar ou colocar a pauta, o ciclo ou uma peça no calendário, na pauta ou "para o Felipe", ou para trocar uma copy, data, horário ou direcionamento de uma postagem já pautada, mesmo sem citar a skill pelo nome.
compatibility: Requer acesso ao banco do artefato do Calendário de conteúdo (no app desktop, a ferramenta ArtifactData; em outros ambientes, a ferramenta Artifact com read_db/write_db). A conta precisa poder editar o artefato.
---

# Exitus - Pautar Ciclos

> Versão 3.0 (19/11/2026) — histórico de mudanças em `CHANGELOG.md`.

O time de marketing é a coordenação (quem pede a pauta) e o Felipe (design). O **Calendário de conteúdo** é onde os dois se organizam: cada postagem aparece no dia em que vai ao ar, com o direcionamento, o texto da peça e a legenda; o Felipe conversa na própria peça, envia a arte nela e muda o estágio. **A pauta fica dentro do calendário, no banco do artefato. Não monte nada no Google Slides.**

Esta skill pega um ciclo **já aprovado** (gerado e refinado com a `exitus-copyright-ciclos`) e grava a pauta no calendário. Ela cuida **só da pauta**: não escreve copy nova e não registra nada no banco de copys (isso é da `exitus-bancodecopys-ciclos`).

Ela só roda com conteúdo aprovado. Se pedirem a pauta antes de aprovar as copys, avise em uma linha e pergunte se quer pautar como está.

- Calendário: `https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC`
- Ferramenta: a do banco do artefato (no app desktop, `ArtifactData`; se ela aparecer só pelo nome, carregue antes). Não republique a página: o calendário lê o banco ao vivo, então a pauta aparece para o Felipe assim que é gravada.

## 1. O que você precisa ter em mãos

Do ciclo aprovado, o **tema** (ex.: "O que é o Exitus") e as datas de início e fim. Se não estiver claro na conversa nem no banco de copys, pergunte.

Para cada conteúdo (topo, meio, fundo ou branding) e cada peça avulsa aprovada (data comemorativa, destaque):

- nome, etapa, formato do feed (Carrossel ou Estático), data e horário
- **Feed**: direcionamento, texto por tela e legenda
- **Story**: um registro por story, com nome, data, horário, direcionamento e texto
- **LinkedIn**: legenda (a arte é a mesma do feed; direcionamento só se for diferente)
- **Canal (Telegram)**: direcionamento da adaptação e legenda

Use sempre a **versão final refinada pela pessoa**, não a sua sugestão original.

**De onde tirar:** primeiro da conversa. Se o ciclo não foi aprovado nesta conversa, carregue a `exitus-bancodecopys-ciclos` e leia a entrada do ciclo no banco de copys do mês. Se o ciclo não estiver no banco nem na conversa, peça as copys aprovadas em vez de inventar. Se algo essencial faltar, pergunte só isso.

Os cabeçalhos da copy e do banco vêm como `Canal — Nome do conteúdo | Formato | Funil | Horário — DD.MM`: daí saem canal, título, formato, etapa, hora e data de cada postagem.

## 2. Formato dos registros

### Ciclo — coleção `ciclos`
- **id**: mês abreviado em minúsculas, sem acento, + dia de início. Ex.: `dez-07`, `jan-04`.
- campos: `inicio` (`AAAA-MM-DD`), `fim` (`AAAA-MM-DD`), `datas` (texto curto, ex.: `07 a 12 de dezembro` ou `30/11 a 12/12`), `nome` (o tema do ciclo).

### Postagem — coleção `posts` (uma por peça e canal)
- **id**: `AAAA-MM-DD_canal_slug-do-titulo`. Canal é `feed`, `story`, `reels`, `linkedin` ou `canal`; o slug é o título em minúsculas, sem acento, com hífens, até 40 caracteres. Ex.: `2026-12-07_feed_o-professor-no-centro`. Se dois posts gerarem o mesmo id, acrescente `-2`.
- campos:
  - `ciclo`: id do ciclo
  - `data`: `AAAA-MM-DD`; `hora`: ex. `12h`, `17h30`
  - `canal`: `feed` | `story` | `reels` | `linkedin` | `canal` (Reels é o vídeo vertical do Instagram; não grave reels como `feed`)
  - `formato`: `Carrossel` | `Estático` (story, reels e canal podem ficar vazios)
  - `etapa`: `Topo` | `Meio` | `Fundo` | `Branding` | nome da data comemorativa
  - `titulo`: nome do conteúdo (no story, o nome do story)
  - `status`: `Em produção` ao pautar (o time muda o estágio no calendário)
  - `direcionamento`: a descrição visual completa para o designer
  - `texto`: lista de `{ "rotulo": "Tela 1 (capa)", "texto": "…" }`, uma entrada por tela ou bloco de texto da arte (`Título`, `Texto corrido`, `CTA` também valem)
  - `legenda`: a legenda completa, com hashtags
  - `observacao`: recado curto para o designer (opcional; aparece em destaque na peça)

## 3. Passo a passo

1. **Confira o que já existe.** Consulte `posts` filtrando `ciclo == <id>` e leia `ciclos/<id>`. Se o ciclo já estiver pautado, não duplique: pergunte se é para substituir ou só ajustar o que mudou.
2. **Grave o ciclo** (`set` em `ciclos`).
3. **Grave as postagens** num lote (`batch`, até 50 escritas por lote; acima disso, vários lotes).
   - Documento novo: `set` sem versão.
   - Documento que já existe: leia antes e use `update` com a versão lida.
4. **Confira**: consulte `posts` pelo ciclo e compare a quantidade com o que foi aprovado (ex.: 3 feeds + 3 LinkedIn + 3 canal + stories).
5. **Avise** a pessoa em poucas linhas: ciclo, quantas postagens por canal, e que o Felipe já vê tudo no calendário (com o link). Lembre que a arte é enviada por ele na própria postagem.
6. Acione a `exitus-bancodecopys-ciclos` para registrar no banco de copys o link do **calendário** (no lugar do antigo link da pauta no Slides).

## 4. Ajustes depois de pautado

- Trocar copy, legenda, direcionamento, data ou horário: leia o post e use `update` com a versão lida. Se a **data** ou o **canal** mudar, o id muda: crie o post com o id novo e apague o antigo, avisando que a conversa daquela peça fica no id antigo.
- Nunca grave nas coleções `estagios`, `conversa` ou `artes`: são do time (estágio, conversa entre coordenação e design, artes enviadas).
- Nunca apague postagens de ciclos já postados sem pedirem.

## 5. Regras de acesso

`posts` e `ciclos` só aceitam escrita de quem edita o artefato (a coordenação, e o Claude agindo por ela). O Felipe, como colaborador, lê a pauta, conversa, muda o estágio e envia artes, mas não altera a pauta. Uma escrita recusada nessas coleções significa que a conta não tem permissão de edição no calendário: avise a pessoa em vez de tentar outro caminho (nunca volte a montar no Slides para contornar).
