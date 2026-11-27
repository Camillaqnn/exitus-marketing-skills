# Changelog — exitus-bancodecopys-ciclos

Mais recente no topo. Ajuste pontual (texto, regra) sobe o número depois do ponto (1.0 → 1.1); mudança de fluxo ou de etapa sobe o primeiro número (1.x → 2.0).

## 1.3 — 19/11/2026
**Alterado**
- A pauta passou do Google Slides para o **Calendário de conteúdo**: a linha "Pauta:" do ciclo guarda o link do calendário, e não mais o nome e o link da apresentação.
- Versões finais vindas de fora: a referência é a postagem no calendário (Direcionamento, Texto da peça, Legenda); os quadros do Slides valem só para pautas antigas.

## 1.2 — 10/11/2026
**Adicionado**
- Conferência antes de registrar: se a etapa já está no doc, não registra de novo (evita duplicar quando a aprovação aciona a skill por dois caminhos).
- A `exitus-pautar-ciclos` aciona esta skill ao terminar a pauta, e a linha "Pauta:" ganha o link.
- Esta skill fornece as versões aprovadas de um ciclo para a pauta quando elas não estão na conversa.
- `update_file` na compatibilidade (usado para mover pasta de mês solta).

**Alterado**
- Cabeçalho das peças no mesmo padrão da `exitus-copyright-ciclos`: `Canal — Nome do conteúdo | Formato | Funil | Horário — DD.MM`.

## 1.1 — 10/11/2026
**Alterado**
- Pasta do banco ganha nível de ano, no mesmo padrão das pautas: `Arquivos > Banco de copys > AAAA > NN - Mês`. A skill cria a pasta do ano quando ela não existe e move para dentro dela uma pasta de mês que esteja solta em `Banco de copys`.
- Na virada do ano, os Aprendizados vêm do doc de dezembro do ano anterior.
- IDs atualizados: pasta `2026` do banco e doc de novembro, que já tem os Aprendizados e o ciclo de 16 a 21/11 migrados do doc antigo.

## 1.0 — 10/11/2026
**Adicionado**
- Skill nova, separada da exitus-copyright-ciclos: registra as copys aprovadas a cada etapa (topo, meio, fundo e peças de data) num Google Doc por mês em `Criação Marketing Exitus > Arquivos > Banco de copys > NN - Mês`.
- Cria a pasta e o doc do mês quando não existem, levando os Aprendizados do mês anterior.
- Estrutura fixa do doc (Aprendizados no topo, um ciclo por Título 1, uma etapa por Título 2, uma peça por Título 3) e bloco "O que mudou da sugestão para a versão final".
- Regras para ajuste de peça já registrada e para versões finais coladas ou lidas da pauta.
