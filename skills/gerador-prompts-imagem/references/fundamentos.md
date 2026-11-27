# Fundamentos da IA para criativos

Glossário da Human Academy — vocabulário técnico usado ao explicar conceitos ou ao escolher termos precisos num prompt/resposta para a Camilla.

## Como a IA pensa
- **Modelo** — o "motor" treinado que gera o resultado. Aprendeu padrões a partir de exemplos e transforma uma entrada (texto, imagem) numa saída. Não é um programa com regras fixas — estima a resposta mais provável.
- **Prompt** — a instrução dada ao modelo (texto, imagem, ou os dois). A qualidade do prompt pesa mais no resultado do que a "potência" do modelo.
- **Token** — a menor unidade que o modelo lê e gera (pedaço de palavra). Custo, velocidade e limites de tamanho são medidos em tokens.
- **Parâmetros** — os ajustes internos que o modelo aprendeu no treinamento.
- **Treinamento** — mostrar exemplos ao modelo até ele aprender padrões.
- **Inferência** — o momento em que o modelo gera de fato uma resposta a partir do prompt.
- **Janela de contexto** — quanto o modelo consegue "lembrar" de uma vez (tudo que foi escrito + respondido).
- **Alucinação** — quando o modelo gera algo plausível mas falso. Por isso saída de IA pede revisão, não confiança cega.

## Tipos de modelo
- **LLM** — modelo de linguagem (texto). Ex: ChatGPT, Claude, Gemini. Ideal para ideação, copy, roteiro, análise.
- **Modelo de imagem** — gera imagem a partir de texto e/ou outra imagem. Ex: Nano Banana, GPT Image, Midjourney.
- **Modelo de vídeo** — gera vídeo a partir de texto, imagem e/ou vídeo. Ex: Seedance, Kling, Veo.
- **Multimodal** — lida com mais de um tipo de mídia ao mesmo tempo (texto + imagem + áudio).

## Ecossistema (onde o trabalho acontece)
- **Hub** — ambiente único que reúne vários modelos no mesmo lugar (ex: Flora, um canvas infinito).
- **Node** — cada bloco de um canvas (um nó gera imagem, outro vídeo, outro texto — a saída de um liga na entrada do outro).
- **Workflow** — a sequência de etapas que leva do briefing à entrega. Um bom workflow é repetível.
- **Plataforma** — o serviço que dá acesso a um ou vários modelos por interface. Não é o modelo — é a porta de entrada.
- **Interface** — a tela onde se clica.
- **API** — a "porta dos fundos" que conecta o modelo a outros sistemas por código; entra quando se automatiza produção em escala.
- **Plugin** — complemento que adiciona IA a um programa já em uso (navegador, editor, suíte de design).
- **MCP** — padrão que permite a assistentes/agentes "chamar" ferramentas e fluxos como comandos.
- **Créditos** — unidade que a maioria das ferramentas cobra por geração. Vídeo custa muito mais crédito que imagem — daí a importância de acertar o prompt antes de gerar.

## Vocabulário de imagem
- **Text-to-image** — gerar imagem a partir de descrição em texto.
- **Image-to-image** — gerar nova imagem usando outra como base/referência. Chave da consistência e da maioria das edições sérias.
- **Upscale** — aumentar resolução mantendo (ou recriando) detalhe. Etapa de pós-produção, não de geração.
- **Inpainting** — editar só uma parte da imagem (apagar/trocar um objeto) sem refazer o resto.
- **Outpainting** — expandir a imagem além das bordas originais, com o modelo "inventando" o entorno.
- **Seed** — valor que define a "semente" de aleatoriedade da geração (permite reproduzir/variar resultados).
- **Aspect ratio** — relação largura/altura (1:1, 9:16, 16:9). Definir antes de gerar evita corte e desperdício de crédito.
- **Resolução** — nível de detalhamento da imagem.
