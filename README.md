# Documentação e Análise Estratégica do Agente Okabooks

## 1. Contexto do Projeto e Objetivo da IA

O projeto Okabooks, da empresa Afluente Arte, visa transformar as memórias de um usuário em um livro biográfico. O processo começa com uma IA Entrevistadora que coleta histórias através de um chat. O objetivo desta IA, a Escritora, é atuar como um biógrafo profissional. Sua função é receber os dados brutos da entrevista (perguntas e respostas) e transformá-los em um capítulo de livro coeso, eloquente e emocionante, mantendo total fidelidade às memórias do usuário enquanto eleva a qualidade da narrativa.

## 2. Visão Geral das Seções do Project Knowledge

Cada seção do Project Knowledge foi desenhada para construir, de forma incremental, a capacidade e o comportamento da IA.

| Seção | Conteúdo e Objetivo |
| :--- | :--- |
| 1. Overview | Apresenta a missão central da IA: transformar dados de entrevista em capítulos biográficos, estabelecendo suas responsabilidades primárias. |
| 2. AI Persona & Capabilities | Define a "personalidade" da IA (biógrafa e editora literária) e lista suas ferramentas (mysql_tool, tavily-mcp, yolo-service, etc.), explicando o que cada uma faz. |
| 3. Core Operating Principles | Estabelece as regras de ouro, inegociáveis, que governam todas as ações da IA, como o uso obrigatório do pensamento sequencial e a fidelidade aos dados. |
| 4. Workflows | Descreve o passo a passo operacional para a principal tarefa da IA: /generate_chapter. Mapeia os caminhos desde a coleta de dados (via input direto ou banco de dados) até a escrita. |
| 5. Writing Style Guide | É o coração do prompt. Contém todas as regras detalhadas sobre voz, tom, estilo, estrutura, e o tratamento de conteúdos específicos (imagens, citações, etc.). |
| 6. Examples & Best Practices | Fornece exemplos de "certo e errado" para materializar as regras abstratas da seção 5, ensinando a IA por meio de demonstrações práticas. |
| 7. Quality Control | Institui um processo de auto-revisão, forçando a IA a usar uma checklist para garantir que o capítulo final cumpra todas as regras antes de ser entregue. |
| 8. Error Handling & Troubleshooting | Prepara a IA para lidar com problemas comuns (dados faltantes, erros de conexão) e define procedimentos de fallback para garantir que ela não trave ou invente informações. |
| 9. Quick Reference | Funciona como uma "cola" ou um resumo de acesso rápido para os comandos, queries e regras mais essenciais, otimizando a performance em tempo de execução. |

## 3. Análise das Ferramentas (Capabilities)

As ferramentas são as capacidades externas da IA, permitindo que ela interaja com o mundo exterior para coletar informações e estruturar seu raciocínio.

### 1. mcp-server-mysql
* **O que faz:** Permite que a IA se conecte e execute consultas em um banco de dados MySQL. É a principal ponte entre a IA e os dados do projeto.
* **Decisão Estratégica:** Em vez de depender de dados inseridos manualmente a cada execução, a IA pode acessar diretamente a fonte de verdade (o banco de dados do projeto). Isso garante que ela trabalhe com as informações mais recentes e completas, incluindo respostas da entrevista, detalhes do projeto (tipo, gênero) e memórias adicionais, automatizando e escalando o processo.

### 2. Tavily e Brave Search
* **O que fazem:** São ferramentas de pesquisa na web. Permitem que a IA busque informações sobre eventos históricos, culturais, ou características de locais mencionados pelo usuário.
* **Decisão Estratégica:** Para cumprir a regra de Contextualização Histórica e Geográfica, a IA precisa de acesso a conhecimento externo. Essas ferramentas fornecem a matéria-prima para enriquecer a narrativa. A existência de duas opções (Tavily para pesquisa aprofundada, Brave para buscas mais amplas) dá flexibilidade e robustez ao processo de pesquisa.

### 3. sequential-thinking
* **O que faz:** É uma ferramenta interna que força a IA a descrever seu plano de ação passo a passo antes de executá-lo.
* **Decisão Estratégica:** É uma salvaguarda de confiabilidade. Ao obrigar a IA a "pensar em voz alta", garantimos que ela siga a lógica correta definida nos workflows. Isso torna seu comportamento mais previsível, auditável e menos propenso a erros ou desvios do processo estabelecido.

### 4. yolo-service
* **O que faz:** Analisa uma imagem a partir de uma URL e retorna uma lista de objetos e classificações de cena detectados.
* **Decisão Estratégica:** Resolve um problema prático e comum: o usuário envia uma imagem sem descrevê-la. Para que a IA possa integrar essa imagem de forma inteligente na narrativa, ela precisa de algum contexto. O yolo-service atua como um fallback, gerando uma descrição básica ("uma foto de uma pessoa e um cachorro em um parque") que permite à IA posicionar a imagem no parágrafo mais relevante.

## 4. Ferramenta de Validação de Qualidade (ContentValidator)

Esta seção descreve o script em Python desenvolvido para analisar e validar a qualidade do capítulo gerado pela IA, garantindo que ele cumpra os princípios fundamentais de fidelidade e completude.

* **O que faz:** O script `ContentValidator` compara o texto original das respostas do usuário (de um arquivo CSV) com o capítulo final gerado pela IA. Ele não busca uma correspondência exata, mas sim uma equivalência semântica, produzindo um relatório detalhado sobre a cobertura do conteúdo.

* **Principais Funcionalidades:**
    * **Análise de Cobertura por Conceitos:** Em vez de procurar frases idênticas, o script extrai "conceitos-chave" (palavras e pequenas frases importantes) das respostas originais e verifica quantos desses conceitos estão presentes no capítulo final. Isso permite que a IA reformule e embeleze o texto, desde que a essência da memória seja mantida.
    * **Análise de Similaridade:** Calcula um score que mede o quão próximo o texto gerado está do original, ajudando a identificar se uma memória foi completamente omitida ou apenas muito reformulada.
    * **Análise de Expansão:** Compara a contagem de caracteres do material original com a do capítulo gerado. Isso ajuda a avaliar se a IA foi concisa demais (comprimindo muito a história) ou prolixa demais (adicionando informações desnecessárias).

* **Decisão Estratégica:** A regra mais importante do prompt é a fidelidade total às memórias do usuário (Rule 19 e Rule 20). No entanto, verificar isso manualmente em dezenas de respostas e um longo capítulo é demorado e sujeito a erros. O `ContentValidator` automatiza essa verificação. Ele fornece uma métrica objetiva e escalável para garantir que a IA não "esqueça" ou invente informações, sendo uma ferramenta de controle de qualidade essencial para o projeto.

## 5. Análise Detalhada das Features do Capítulo

Estas são as características principais que o prompt foi desenhado para garantir em cada capítulo, transformando uma lista de respostas em uma narrativa de qualidade.

### 1. Estrutura Crono-Temática
* **O que faz:** Organiza as memórias do usuário em uma linha do tempo lógica. Eventos com datas são postos em ordem cronológica, e memórias sem data são agrupadas por tema e encaixadas nos pontos mais relevantes dessa linha do tempo.
* **Decisão Estratégica:** Evitar que o capítulo seja uma simples transcrição das respostas na ordem em que foram dadas. Isso cria um fluxo de leitura natural e profissional, onde uma ideia leva à outra, em vez de uma lista desconexa de fatos. É o pilar para transformar um "Q&A" em uma "narrativa".
* **Comprovação no Prompt:**
    * **Regra 28 (Structure and Flow):** `Rule 28: Chrono-Thematic Structuring Process` detalha explicitamente o processo: "List dated events in chronological order -> Cluster undated memories by theme -> Attach each theme group to the nearest dated event."

### 2. Contextualização Histórica e Geográfica
* **O que faz:** Enriquece a narrativa adicionando, de forma sutil, breves descrições do contexto histórico da época ou de características conhecidas dos locais mencionados pelo usuário.
* **Decisão Estratégica:** Situar a história pessoal do biografado em um mundo maior, adicionando cor e profundidade. A regra de ser sutil e apolítico (máx. 10 palavras) garante que o contexto enriqueça a memória, sem nunca ofuscá-la ou se tornar o foco principal.
* **Comprovação no Prompt:**
    * **Regra 0 (Language and Style):** `Rule 0: Context Integration` define processos separados para `Historical Context` e `Location Context`, com instruções claras: "DO NOT let it have bigger protagonism", "Be Apolitical", "Synthesize... into a concise and objective description (max: 10 words)".
    * **Workflows (Block 2, Steps 4 & 5):** Os passos `Research Historical Context` e `Research Location Context` formalizam essa pesquisa como parte do processo obrigatório.

### 3. Integração de Memórias "Órfãs"
* **O que faz:** Identifica fatos pequenos e isolados (apelidos, medos de infância, hobbies) que não têm uma conexão cronológica óbvia e os integra de forma fluida no corpo do texto, em vez de listá-los no final.
* **Decisão Estratégica:** Garante que toda informação fornecida pelo usuário seja utilizada (um princípio fundamental do prompt) e adiciona textura e personalidade à narrativa. Evita o erro comum de criar um parágrafo "lista de curiosidades" no final, que quebra a imersão.
* **Comprovação no Prompt:**
    * **Regra 32:** `Identifying Orphan Memories` define o que são.
    * **Regra 33:** `Logical Anchoring of Orphan Memories` proíbe que sejam simplesmente anexadas no final e exige que se encontre um "ponto de âncora" lógico no texto.
    * **Regra 34:** `Narrative Weaving with Thematic Bridges` torna obrigatório o uso de uma "ponte temática" para justificar a inserção da memória, integrando-a a um parágrafo existente.

### 4. Aberturas e Fechamentos Impactantes
* **O que faz:** Exige que a IA não comece o capítulo simplesmente respondendo à primeira pergunta, mas sim criando um parágrafo de abertura que introduza o tema central do capítulo. Da mesma forma, o final deve ser uma reflexão que conclui o tema, e não apenas o último fato.
* **Decisão Estratégica:** Capturar o interesse do leitor desde a primeira linha e deixar uma impressão duradoura no final. Isso eleva o texto de um simples relato para uma peça literária com começo, meio e fim bem definidos.
* **Comprovação no Prompt:**
    * **Regra 29 (Impactful Chapter Openings):** "Don't start by answering the first question. Identify central theme from all answers. Create engaging opening that introduces the theme."
    * **Regra 35 (Reflective Chapter Closings):** Descreve o processo de identificar o sentimento principal do período e usá-lo para criar uma reflexão final, em vez de "listar fatos restantes".

### 5. Fluxo Narrativo e Conexões Fluidas
* **O que faz:** Garante que as frases e parágrafos se conectem de maneira lógica e orgânica, usando transições que guiam o leitor suavemente pela história.
* **Decisão Estratégica:** Criar uma leitura agradável e imersiva. A ausência de conexões é o que faz um texto parecer robótico ou como uma lista. Essas regras forçam a IA a "pensar" como um escritor, preocupando-se com o ritmo e a cadência do texto.
* **Comprovação no Prompt:**
    * **Regra 6 (Fluid Connections):** "Connect answers naturally. Create narrative flow, not a Q&A list."
    * **Regra 30 (Strategic Paragraphing):** "Do not create a new paragraph for every memory or answer. Group related ideas to form cohesive paragraphs."
    * **Regra 31 (Smooth Paragraph Transitions):** Exige o uso de pontes temáticas, temporais ou de causa e efeito para ligar os parágrafos.

### 6. Tratamento de Citações (Quotes)
* **O que faz:** Define quando e como usar citações diretas. Só são permitidas se o usuário explicitamente as usou (com aspas) ou se são de figuras públicas e foram verificadas. A formatação é padronizada (bloco de citação, itálico).
* **Decisão Estratégica:** Preservar a força de declarações importantes e dar-lhes o devido destaque visual e narrativo. A regra de verificação evita a propagação de citações incorretas.
* **Comprovação no Prompt:**
    * **Regra 36 (When to Use Direct Quotes):** Define as condições estritas para o uso de citações.
    * **Regra 37 (Quotation Formatting):** "Italicized, indented block in its own paragraph."

### 7. Leitura e Integração de Imagens (WhatsApp)
* **O que faz:** A IA consegue identificar URLs de imagens enviadas pelo usuário, analisar o contexto da imagem (pelo texto próximo ou usando uma IA de visão computacional, a yolo-service) e, por fim, inserir a URL da imagem no local mais apropriado do capítulo.
* **Decisão Estratégica:** Integrar perfeitamente o conteúdo visual ao textual, que é uma parte crucial do produto Okabooks. Em vez de apenas listar as imagens no final, a IA as coloca onde elas fazem sentido narrativo, enriquecendo a experiência do leitor. O uso da yolo-service como fallback é uma solução robusta para imagens enviadas sem descrição.
* **Comprovação no Prompt:**
    * **Regra 15:** `Identifying Image URLs` define o padrão da URL.
    * **Regra 16:** `Determining the Image Context` explica como procurar por descrições.
    * **Regra 17:** `Context Fabrication using YOLO` detalha o processo de fallback para gerar uma descrição.
    * **Regra 18:** `Intelligent Placement Image URLs` comanda a inserção da imagem em um parágrafo relevante, em vez de no final do capítulo.

### 8. Processo de Revisão e Controle de Qualidade
* **O que faz:** Após escrever o primeiro rascunho, a IA é forçada a "vestir o chapéu" de um editor literário e usar uma checklist extensa para revisar seu próprio trabalho em busca de violações das regras.
* **Decisão Estratégica:** Garantir a consistência e a alta qualidade da entrega. Em vez de confiar que a primeira versão será perfeita, este processo de auto-correção aumenta drasticamente a chance de o resultado final estar em conformidade com todas as dezenas de regras definidas.
* **Comprovação no Prompt:**
    * **Seção 7 (Quality Control):** A seção inteira é dedicada a isso, com o `Self-Review Process` e a `Editorial Checklist`.
    * **Workflows (Block 2, Step 8):** `Editorial Review` é um passo obrigatório no fluxo de trabalho.

## 6. Pilares da Escrita de Qualidade: Tom, Voz e Regras Essenciais

Para garantir que o capítulo seja "bem escrito", o prompt foca em três pilares principais que governam o tom e o estilo.

### 1. A Voz do Biografado (Autenticidade e Fidelidade)
O objetivo não é criar uma voz genérica, mas sim capturar e polir a voz do usuário.
* **Voz Condicional:** A regra mais básica (`5.1 Voice & Tone`) define se o texto será em 1ª pessoa ('autobiography') ou 3ª pessoa ('biography_other_person'), garantindo a perspectiva correta.
* **Polimento, não Substituição:** As regras de `Language and Style` (`5.2`) instruem a IA a substituir gírias ou informalidades excessivas por uma linguagem mais polida, mas sem perder a essência. O exemplo de "deu um duro danado" para "trabalhou incansavelmente" é a chave: o significado é mantido, a formalidade é elevada.
* **Interpretação de Nuances:** A `Regra 24 (Interpretation of Nuance and Intent)` é crucial. Ela comanda a IA a não ser literal, a entender o significado implícito e a capturar a intenção do usuário, o que resulta em um texto muito mais humano e menos robótico.

### 2. A Mão do Editor (Clareza, Ritmo e Precisão)
Aqui estão as regras que garantem que o texto seja tecnicamente bom e agradável de ler.
* **Concissão e Força:** A `Regra 3 (Conciseness)` e a `Regra 5 (Active Voice Priority)` trabalham juntas para criar um texto direto e enérgico, evitando a passividade e o excesso de "enchimento".
* **Ritmo e Variedade:** A `Regra 4 (Varied Sentence Structure)` combate a monotonia, forçando a IA a alternar entre frases curtas e longas e a não iniciar parágrafos sempre da mesma forma. Isso é fundamental para uma prosa de qualidade.
* **Linguagem Natural e Precisa:** A `Regra 2 (Natural Language)` proíbe clichês ("jornada", "turbilhão de emoções"), enquanto a `Regra 9 (Semantic Precision of Verbs)` impede que a IA use palavras que soam "bonitas" mas são incorretas no contexto (o exemplo de datilografar vs. tocar a máquina de escrever é perfeito).

### 3. A Neutralidade do Biógrafo (Imparcialidade e Respeito)
Este pilar garante que a IA lide com temas complexos de forma ética e responsável.
* **Neutralidade Política:** A `Regra 1 (Political Neutrality)` impede que a IA insira termos politicamente carregados, especialmente no contexto histórico, garantindo que o foco permaneça na experiência pessoal do usuário, e não em uma análise política.
* **Tratamento de Temas Sensíveis:** A `Regra 25 (Careful Handling of Sensitive Topics)` exige um tom "excepcionalmente empático" e a reformulação de declarações diretas para uma forma narrativa mais respeitosa, como demonstrado no exemplo sobre o pai generoso.
* **Exclusão de Conteúdo Discriminatório:** A `Regra 26` é uma diretriz ética fundamental. Ela ensina a IA a diferenciar entre o relato de uma experiência com preconceito (que deve ser incluida) e a expressão de um preconceito pelo usuário (que deve ser silenciosamente removida), protegendo a integridade e o caráter respeitoso da obra final.