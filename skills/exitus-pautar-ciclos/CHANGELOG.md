# Changelog — exitus-pautar-ciclos

Mais recente no topo. Ajuste pontual (texto, regra) sobe o número depois do ponto (1.1 → 1.2); mudança de fluxo ou de etapa sobe o primeiro número (1.x → 2.0).

## 3.1 — 21/11/2026
**Adicionado**
- Canal `reels` (vídeo vertical do Instagram) nas postagens, com cor e ícone próprios no calendário. Reels não é mais gravado como `feed`.

## 3.0 — 19/11/2026
**Alterado**
- A pauta sai do Google Slides e passa a ser gravada no **Calendário de conteúdo** (artefato), no banco dele: um ciclo na coleção `ciclos` e uma postagem por peça na coleção `posts`, com data, horário, canal, formato, etapa, direcionamento, texto da peça e legenda. O designer (Felipe) vê a pauta ao vivo, conversa e envia a arte na própria postagem.
- O registro no banco de copys passa a guardar o link do calendário, no lugar do link da pauta no Slides.

**Removido**
- Cópia do modelo de Slides, pastas por ano e mês no drive Criação Marketing Exitus, ajuste de slides e `references/mapa-modelo.md`.
## 2.2 — 19/11/2026
**Alterado**
- Nome da apresentação: de `[EXITUS] Pauta de DD a DD de Mês` para `[PAUTA DE CICLO] Tema - DD Mmm a DD Mmm` (ex.: `[PAUTA DE CICLO] O que é o Exitus - 23 Nov a 05 Dez`), com o mês nos dois lados da data. As pautas anteriores mantêm o nome antigo.

**Adicionado**
- O tema do ciclo entra no que a skill precisa ter em mãos; sem ele, a skill pergunta antes de criar a cópia.

## 2.1 — 10/11/2026
**Adicionado**
- Se o ciclo não foi aprovado na conversa, a pauta busca as versões aprovadas no banco de copys (via `exitus-bancodecopys-ciclos`) em vez de depender do chat.
- Ao terminar a pauta, aciona a `exitus-bancodecopys-ciclos` para colocar o link da pauta no banco.
- Nota sobre o cabeçalho novo (`Canal — Nome | …`): na pauta, o canal sai, porque já é a seção do slide.

## 2.0 — 10/11/2026
**Alterado**
- Pastas: a pauta vai para `Criação Marketing Exitus > AAAA > Cíclo de Postagens > NN - Mês`. A skill cria sozinha a pasta do mês (ex.: `12 - Dezembro`) e, na virada do ano, a pasta do ano com `Cíclo de Postagens` dentro (ex.: `2027 > Cíclo de Postagens > 01 - Janeiro`). Antes ela perguntava e só conhecia as pastas de 2026.
- Foco só na apresentação: o registro no banco de copys saiu daqui e foi para a `exitus-bancodecopys-ciclos`.
- Regra reforçada: todo direcionamento fica escrito na própria apresentação, nunca em comentário.
- Descrição mais abrangente: vale para qualquer pessoa do time e cobre trocar copy numa pauta já montada. No texto, "a usuária" virou "a pessoa".

**Corrigido**
- "Só siga para o passo 3 da montagem" → "siga para a seção 3 (Ajustar a estrutura ao ciclo)".
- Conectores que faltavam na compatibilidade: `read_slide_page_thumbnail` (miniaturas para conferir texto estourado) e `create_file` (criar pastas).

## 1.1 — 10/11/2026
**Corrigido**
- Referência ao banco de copys: aponta para o Google Doc no drive Criação Marketing Exitus e para a seção 8 da `exitus-copyright-ciclos` (antes citava uma "seção 8.1" que não existe).

**Adicionado**
- Linha de versão no topo do SKILL.md e este CHANGELOG.

## 1.0 — antes de 10/11/2026
- Versão inicial: duplica o modelo "[EXITUS] Pauta de Cíclos de Postagem", ajusta datas, nomes, formatos e slides, preenche legendas e escreve o direcionamento nos quadros; ajuste pontual de peça depois da pauta montada.
