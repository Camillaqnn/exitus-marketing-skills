# Estrutura de um prompt de imagem — as 6 camadas

Fonte: Guia "Criação de Imagens com IA — Prompts e Workflows" (Human Academy). Esta é a metodologia principal a seguir sempre que a Camilla pedir ajuda para escrever ou revisar um prompt de geração/edição de imagem.

## A lógica central

O modelo não tem intenção própria: ele preenche tudo o que não foi dito com o resultado mais provável — e "mais provável" quase sempre significa genérico (luz neutra, fundo qualquer, enquadramento de centro).

**A virada de chave**: pare de descrever *o que* você quer ver e comece a dirigir *como* deve ser visto — de que ângulo, sob qual luz, com qual textura, em qual estética. A diferença entre uma imagem boa e uma medíocre raramente está no modelo; está na quantidade de direção dada. Cada detalhe acrescentado é uma decisão criativa tomada em vez de delegada à média.

## As seis camadas

Ao construir ou revisar um prompt para a Camilla, percorrer estas seis camadas na ordem, garantindo que cada uma esteja especificada com detalhe concreto (não genérico):

1. **Sujeito e ação** — quem ou o quê, fazendo o quê. O ponto focal e sua ação na cena.
2. **Enquadramento e ângulo** — o plano, o ângulo, a lente. A primeira decisão de câmera (ex: ultra-wide contra-plongée, câmera no chão, diagonais agressivas, horizonte inclinado).
3. **Luz e atmosfera** — direção, qualidade e horário da luz. Define atmosfera e leitura visual (ex: luz lateral natural 5700K, sombras suaves, reflexos vivos).
4. **Cenário e detalhe** — o ambiente concreto. Detalhe específico vence o genérico sempre (ex: campo de golfe, marcas curvas de pneus na grama, texturas detalhadas).
5. **Estilo e referência** — a natureza da imagem e uma âncora estética (uma corrente, um tipo de filme, uma década). Dá ao modelo onde se apoiar (ex: editorial anos 2000, color blocking, estética jovem e documental).
6. **Técnico** — proporção, resolução, acabamento. Detalhes que evitam corte e desperdício de geração.

## Exemplo de aplicação (nível de detalhe esperado)

Um prompt bem construído nessa metodologia não é "um homem dirigindo um carrinho de golfe" — é algo como:

> **Sujeito e Ação**: Homem de 20–30 anos dirige um carrinho de golfe em alta velocidade, sentado diagonalmente, pernas afastadas, corpo inclinado numa curva. Uma mão enluvada no volante; a outra segura um MacBook aberto pra fora do veículo. Olha pro laptop, ignorando o caminho, expressão concentrada e travessa. Garantir anatomia convincente e peso realista do laptop.
>
> **Enquadramento e Ângulo**: Retrato de ação ultra-wide em contra-plongée, câmera no espaço dos pés do passageiro, 0,25–0,35m do piso. Inclinada pra cima e diagonalmente. Diagonais agressivas, perspectiva perto-longe acentuada, horizonte inclinado.
>
> **Luz e Atmosfera**: Luz natural lateral intensa (~5700K), sombras faciais suaves, sem luz frontal artificial. Pele em 68–76 IRE, metais/nuvens em 88–94 IRE, sem estouro.
>
> **Cenário e Detalhes**: Campo de golfe bem cuidado, caminho pavimentado curvo, céu amplo com nuvens.

Note o nível de precisão técnica: distâncias em metros, temperatura de cor em Kelvin, exposição em IRE. Isso é o padrão a mirar quando a Camilla pedir um prompt "profissional" ou "no nível dos exemplos da Human Academy" — não é obrigatório em todo prompt, mas é a régua de "prompt bem construído" nessa metodologia.

## Ao ajudar a Camilla a escrever um prompt

- Perguntar (ou inferir do contexto) o que falta nas 6 camadas antes de entregar o prompt final.
- Preferir especificidade concreta a adjetivos vagos ("bonito", "profissional") — trocar por decisão técnica (tipo de luz, ângulo, textura).
- Se o pedido for edição (não geração do zero), ver `nano-banana-prompts.md` para os padrões específicos de edição via referência.
- Idioma: escrever sujeito/ação/enquadramento/cenário em português; vocabulário técnico de câmera/luz/textura em inglês; texto que vai renderizar na imagem entre aspas, na língua final desejada. Ver `nano-banana-oficial.md` para o raciocínio completo por trás disso.
