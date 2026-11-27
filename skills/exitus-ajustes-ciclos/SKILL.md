---
name: "exitus-ajustes-ciclos"
description: Transforma pedidos de ajuste nas peças dos ciclos de postagem do Exitus em ajustes pautados no Calendário de conteúdo (artefato https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC). Lê o pedido (da diretoria, do aprovador ou da revisão da coordenação), encontra as peças afetadas e as variações delas, escreve a proposta de cada ajuste e mostra a lista numerada para o time escolher quais entram. Depois grava os escolhidos na caixa "Ajustes" de cada peça, atualiza copy e legenda na pauta e muda o estágio da peça para "Ajuste". Use SEMPRE que alguém colar, encaminhar ou contar um pedido de ajuste, correção, mudança ou feedback sobre peças, copys, legendas ou artes de um ciclo, quando a coordenação revisar um ciclo e apontar o que precisa mudar, ou quando pedirem para pautar, registrar, colocar como comentário ou mandar ajustes para o designer, mesmo sem citar a skill pelo nome.
compatibility: Requer acesso ao banco do artefato do Calendário de conteúdo (no app desktop, a ferramenta ArtifactData; em outros ambientes, a ferramenta Artifact com read_db/write_db). A conta precisa poder editar o artefato.
---

# Exitus - Ajustes dos Ciclos

> Versão 1.0 (24/11/2026) — histórico de mudanças em `CHANGELOG.md`.

Qualquer pessoa do time pode pedir. Nesta skill, "a pessoa" é quem está na conversa (em geral a coordenação de marketing); "quem pediu" é quem fez o pedido de ajuste (ex.: o Pedro, da diretoria).

No fluxo do marketing, uma peça reprovada vira **ajuste** (alterar só uma parte; vai para a redação ou para o design) ou **refação** (refazer a peça). Esta skill cuida dos **ajustes**: transforma o pedido em itens claros, deixa o time escolher quais entram e pauta cada um no **Calendário de conteúdo**, na caixa "Ajustes" da peça, onde o designer vê, marca como feito e é avisado no sino.

Ela não escreve ciclo novo (`exitus-copyright-ciclos`), não monta a pauta do zero (`exitus-pautar-ciclos`) e não mexe no banco de copys por conta própria: no fim, aciona a `exitus-bancodecopys-ciclos`.

- Calendário: `https://claude.ai/artifact/XayrXmzDmBsL5qftyNVrNC`
- Ferramenta: a do banco do artefato. Não republique a página: o calendário lê o banco ao vivo.
- Antes de escrever qualquer texto novo, carregue a `exitus-voz-e-marca` (e, para copy de peça e legenda, as regras de escrita da `exitus-copyright-ciclos`).

## 1. Entender o pedido

O pedido pode chegar colado no chat, em print, em áudio transcrito, como lista numerada que acompanha as peças, ou a pessoa pode estar revisando e apontando o que mudar.

1. **Ache o ciclo e as peças.** Consulte `posts` filtrando `ciclo == <id>` (o id do ciclo está em `ciclos`). Peças são identificadas pelo título e pela data; se a lista do pedido for numerada ("1, 2, 3…"), em geral cada número é um post de feed na ordem do ciclo. Se não der para saber qual peça é, pergunte.
2. **Estenda para as variações.** Um pedido sobre o feed quase sempre vale para o LinkedIn, o Telegram e os stories do mesmo conteúdo ("os outros são variações do feed"). Procure o mesmo trecho em todas as peças do ciclo e inclua cada uma que repete o problema.
3. **Classifique cada ponto** pelo tipo da caixa de ajustes:
   - `texto` (Texto da arte): o que está escrito dentro da imagem.
   - `imagem` (Imagem/layout): cor, foto, composição, elementos visuais.
   - `legenda` (Legenda): texto da legenda do post.
   - `direc` (Direcionamento): mudança na descrição visual que orienta o designer.
   - `outro`: data, horário, formato ou o que não couber acima.
4. **Ponto aprovado** ("ok", "gostei") não vira ajuste. **Pedido de mais conteúdo no mesmo sentido** ("precisamos de mais posts sobre isso") é estratégia para os próximos ciclos: anote na resposta, não pauta.
5. **Refação**: se o pedido derruba a peça inteira (outro tema, outra ideia), diga que é refação e pergunte se é para tratar assim antes de seguir.

## 2. Escrever a proposta de cada ajuste

- **Texto e legenda**: escreva a versão nova exata, pronta para usar, mudando só o que o pedido pede. Quando o pedido é vago ("o texto pode melhorar"), proponha uma versão e diga em meia linha o que mudou.
- **Imagem e direcionamento**: escreva a instrução para o designer em uma ou duas frases, objetiva.
- **Confira fatos.** Se o ajuste envolve um dado (ranking, número, lei, regra), pesquise antes. A peça não pode contradizer a fonte oficial citada no pedido (ex.: o Instagram do Christus). Quando não tiver a frase exata da fonte, diga isso e proponha uma versão que não contradiga nada.
- **Regras que já pegaram ajuste e valem sempre**: nada de datas relativas ("na sexta", "nesta semana") em peça cuja data pode mudar; "Enem" já é nacional, sem "do Brasil"; IA não corrige nem dá nota (o Exitus **auxilia na correção** e o professor avalia); "Ambiente Educacional Completo" sempre como nome próprio.

## 3. Mostrar a lista e deixar o time escolher

Antes de gravar qualquer coisa, mostre os ajustes numerados, agrupados por peça e na ordem das datas:

```
1. Feed 24/11 — Antes da tecnologia, existe a educação · Texto da arte
   Pedido: tirar "forma gente".
   Antes: O Exitus nasceu dentro do Colégio Christus, que forma alunos em Fortaleza desde 1951.
   Depois: O Exitus nasceu dentro do Colégio Christus, que educa gerações em Fortaleza desde 1951.
```

Feche com uma pergunta só: quais números entram (ou "todos"). A pessoa pode mudar textos, juntar ou tirar itens; refaça só o que ela apontou e mostre de novo apenas o que mudou. **Nada é gravado sem a escolha da pessoa.**

## 4. Gravar no calendário

Com a seleção confirmada, leia de novo as peças envolvidas (para ter a versão atual) e grave tudo num lote (`batch`, até 50 escritas; acima disso, vários lotes):

1. **Copy na pauta.** Para ajustes de `texto`, `legenda` e `direc`, atualize o campo da peça em `posts` (`texto`, `legenda` ou `direcionamento`) com `update` e a versão lida, para a pauta já mostrar a versão nova.
2. **Um ajuste por peça e por tipo** na coleção `ajustes`:
   - id: `aj-AAAA-MM-DD-canal-tipo` (data e canal da peça; acrescente `-2`, `-3` se repetir).
   - campos: `post` (id da peça), `tipo`, `texto`, `para`, `de`, `prazo` (`AAAA-MM-DD` se a pessoa der um prazo; senão vazio), `feito`, `em` (agora, ISO).
   - `texto`: começa com quem pediu e o motivo, e termina com a instrução exata. Ex.: `Pedido do Pedro (diretoria): não citar 2º e 3º lugar. Trocar o texto corrido da tela 2 por: "Christus Pré-Universitário: 1º lugar no Enem em 2024 e 2025."` Se o ajuste veio da revisão da própria coordenação, use `Revisão da coordenação:`.
   - `para`: quem executa. Texto da arte, imagem e direcionamento vão para o designer; copie o `para` de ajustes recentes do mesmo tipo no calendário. Se não houver nenhum, peça à pessoa para criar um ajuste para o designer pelo formulário da peça e copie dele. `de`: o mesmo id que o time usa nos ajustes que cria (copie de um ajuste existente).
   - **Legenda**: como você já atualizou a legenda na pauta, grave o ajuste com `feito: true`, `feitoEm` e `feitoPor` (o mesmo valor de `de`), terminando o texto com "Legenda já atualizada na pauta."
   - Texto da arte, imagem, direcionamento e outro ficam com `feito: false`: só o designer marca como feito.
3. **Estágio da peça.** Para cada peça que ficou com ajuste em aberto, grave `estagios/<id da peça>` com `{ "estagio": "ajuste", "em": <agora>, "por": <o mesmo valor de de> }`. Peça que só teve a legenda ajustada mantém o estágio. Em refação confirmada, use `"reprovado"`. Não mude o estágio de peça já postada.
4. **Não use** o campo `observacao` para ajustes (se houver recado antigo do mesmo pedido lá, limpe) e nunca grave em `conversa` ou `artes`.

## 5. Conferir e avisar

1. Leia de volta as peças, os ajustes e os estágios do ciclo e confira: cada item escolhido virou ajuste na peça certa, a copy da pauta está com a versão nova e não sobrou o texto antigo em nenhuma variação.
2. Responda em poucas linhas: quantos ajustes, em quais peças, quais estão com o designer e quais já foram feitos (legendas), e quais peças foram para o estágio "Ajuste". Se algo ficou de fora (estratégia, refação a confirmar), diga em uma linha.
3. Acione a `exitus-bancodecopys-ciclos` para trocar as copys ajustadas no banco do mês e acrescentar o "Ajustado em DD/MM" de cada post.

## 6. Depois de pautado

- Pedido para marcar ajustes como feitos: só os que a pessoa indicar, com `feitoEm` e `feitoPor`.
- Quando todos os ajustes de uma peça estiverem feitos, não mude o estágio sozinho: a aprovação é do time.
- Se a data de uma peça mudar e o id da postagem mudar junto, atualize o campo `post` dos ajustes dela para o id novo.
