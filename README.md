# 🤖 AgentBot Analytics Neymar Jr. Career: Agente Inteligente com IA Generativa

## Contexto

Os assistentes virtuais estão evoluindo de simples chatbots reativos para **agentes inteligentes e proativos**. Neste desafio, você vai idealizar e prototipar um agente consultor que utiliza IA Generativa para:

- **Antecipar necessidades**: Em vez de apenas dizer quantos gols ele fez, o agente correlaciona dados. Se você pergunta sobre "Gols em 2024", ele antecipa a análise de que o baixo volume se deve ao tempo de recuperação da lesão de LCA, sugerindo uma comparação com a média de gols pré-lesão
- **Personalizar**: O agente adapta a resposta conforme o perfil da pergunta. Se o foco é mercado, ele prioriza dados de `transacoes.csv`. Se o foco é saúde, ele personaliza a análise baseada na recorrência de lesões no tornozelo, adaptando o nível de detalhamento técnico.
- **Cocriar soluções**: O agente atua como um parceiro de decisão. Ele cruza o "custo" da transferência com a "entrega" em campo. Exemplo: "Dado o histórico atual, a melhor solução para o ciclo de 2026 é um contrato de produtividade focado em minutos jogados, visando a preservação física.
- **Garantir segurança**: O agente é proibido de inventar estatísticas. Se o dado não consta no `Carreira e Lesões.json`, ele informa que a métrica não está disponível. Além disso, bloqueia qualquer tentativa de extrair dados privados (fofocas), garantindo a ética profissional.

> [!TIP]
> Na pasta [`examples/`](./examples/) você encontra referências de implementação para cada etapa deste desafio.

---

## O Que Você Deve Entregar

### 1. Documentação do Agente

Defina **o que** seu agente faz e **como** ele funciona:

- **Caso de Uso:** O AgentBot Analytics resolve o problema da fragmentação e da falta de confiabilidade em dados esportivos de alto nível.
  Enquanto IAs genéricas costumam "alucinar" estatísticas, este agente utiliza uma base de dados estruturada para fornecer consultoria técnica sobre a carreira de Neymar Jr.,
  focando em performance, histórico médico e inteligência de mercado.
  
- **Persona e Tom de Voz:** 
    Persona: Consultor Técnico e Analista de Mercado Esportivo.
    Comportamento: Objetivo, focado em fatos e estatísticas.
    Tom de Voz: Profissional, analítico e direto. O agente evita adjetivos emocionais e foca em métricas (KPIs).

- **Arquitetura:**
  O projeto utiliza a técnica de RAG (Retrieval-Augmented Generation) com Context Injection:
    Base de Conhecimento: Arquivos CSV e JSON contendo o histórico real (2009- Atual).
    Recuperação: O sistema (via Python/Pandas) filtra os dados relevantes conforme a pergunta.
    Injeção: Os dados são inseridos no Prompt do Sistema como fatos imutáveis.
    Processamento: O LLM processa a informação e gera a resposta técnica.

- **Segurança:** Como evitar alucinações e garantir respostas confiáveis?
  Para garantir a confiabilidade (pilar crítico em projetos de IA Financeira/Dados), o agente segue as regras:
    Filtro de Escopo: Ignora perguntas sobre vida pessoal, família ou fofocas.
    Privacidade: Resposta padrão de "Dados Sigilosos" para qualquer tentativa de acesso a documentos (CPF, contas).
    Fidelidade aos Dados: Se a informação não estiver nos arquivos `data/`, o agente está instruído a admitir que não possui a informação, em vez de inventar números.

📄 **Template:** [`docs/01-documentacao-agente.md`](./docs/01-documentacao-agente.md)

---

### 2. Base de Conhecimento

Utilize os **dados mockados** disponíveis na pasta [`data/`](./data/) para alimentar seu agente:

| Arquivo | Formato | Descrição |
|---------|---------|-----------|
| `transacoes.csv` | CSV | Histórico de transferências, multas rescisórias e salários ao longo da carreira. |
| `historico_atendimento.csv` | CSV |Registro de marcos contratuais, propostas recusadas e decisões estratégicas de carreira. |
| `Prêmio e Títulos.json` | JSON | Perfil técnico do atleta, incluindo conquistas individuais, coletivas e scouting de fundamentos. |
| `Carreira e Lesões.json` | JSON | Base de performance com estatísticas de jogos e histórico médico detalhado. |

Você pode adaptar ou expandir esses dados conforme seu caso de uso.

📄 **Template:** [`docs/02-base-conhecimento.md`](./docs/02-base-conhecimento.md)

---

### 3. Prompts do Agente

Documente os prompts que definem o comportamento do seu agente:

- **System Prompt:**
  Você é o AgentBot Analytics, um especialista em performance esportiva e mercado da carreira de Neymar Jr. Sua função é atuar como um consultor técnico com acesso a dados de mercado e histórico médico.
REGRAS DE OURO:
- Foco Profissional: Limite-se estritamente a dados de carreira (gols, lesões, transferências). Ignore qualquer assunto de vida pessoal ou fofocas.
- Anti-Alucinação: Utilize exclusivamente a base de dados fornecida (CSV/JSON). Se a informação não estiver nos arquivos, informe que não possui esses dados.
- Precisão de Scouting: Diferencie gols de falta, pênaltis e gols de campo conforme os dados técnicos.
- Tom de Voz: Analítico, direto e técnico. Você é uma ferramenta de inteligência, não um chatbot de entretenimento.

- **Exemplos de Interação:**
  Cenário: Consulta de Performance
        Usuário: "Quantos gols ele fez no Barcelona?"
        Agente: "Durante sua passagem pelo FC Barcelona (2013-2017), Neymar Júnior registrou 105 gols em competições oficiais.
                 A análise técnica aponta uma média de 0.56 gols por partida, sendo peça fundamental na conquista da Champions League 14/15."
  Cenário: Análise de Mercado e Saúde
        Usuário: "Qual foi o impacto da lesão de 2023 no contrato dele?"
        Agente: "A ruptura do LCA em outubro de 2023 resultou em um período de inatividade superior a 9 meses.
                 Consultando o histórico de transferências, observa-se que este evento coincidiu com a transição estratégica para o Al-Hilal,
                 onde o valor de mercado foi ajustado conforme a nova realidade física do atleta."
  
- **Tratamento de Edge Cases:** Como o agente lida com situações limite
  Pergunta sobre Vida Pessoal: 
          Resposta: "Sou um agente especializado estritamente na carreira profissional e performance técnica de Neymar Jr. Não possuo acesso a informações de vida pessoal."
  Solicitação de Dados Sigilosos (CPF/Contas):
        Resposta: "Sou o AgentBot, uma IA baseada na busca de dados públicos e técnicos de carreira. Não acesso dados sigilosos, pois viola a lei de privacidade e ética profissional."
Informação Inexistente na Base:
        Resposta: "Esta métrica específica não consta na minha base de dados atual de carreira e performance. Posso ajudar com dados de gols, lesões ou transferências confirmadas?"

📄 **Template:** [`docs/03-prompts.md`](./docs/03-prompts.md)

---

### 4. Aplicação Funcional

Desenvolva um **protótipo funcional** do seu agente:

O protótipo do AgentBot Analytics foi desenvolvido para transformar dados brutos em uma interface de chat inteligente e responsiva, focada na experiência do analista de dados esportivos.

- Chatbot interativo: Desenvolvida em Streamlit, permitindo uma interação fluida e visualização de tabelas e métricas em tempo real.
- Integração com LLM: Integração com Ollama (modelo Llama 3 ou similar) rodando localmente, garantindo privacidade dos dados e baixa latência.
- Conexão com a base de conhecimento: Utilização da biblioteca Pandas para a leitura e filtragem dos arquivos CSV e JSON antes do envio para o modelo de linguagem (técnica de Context Injection).

Componentes do Protótipo

- Chatbot Interativo: Uma interface de chat onde o usuário pode realizar perguntas naturais (ex: "Qual o histórico de gols no Santos?").
- Motor de Busca (RAG): O agente identifica as palavras-chave na pergunta, consulta os arquivos `transacoes.csv` e `Carreira e Lesões.json` e extrai as linhas relevantes.
- Processamento de Resposta: O LLM recebe os dados filtrados como contexto e gera uma resposta analítica seguindo o tom de voz definido no System Prompt.

Como Rodar a Aplicação
Para executar o protótipo localmente, siga os passos abaixo:
```
# 1. Instale as dependências
pip install streamlit pandas ollama

# 2. Certifique-se de que o Ollama está rodando e o modelo baixado
ollama run llama3

# 3. Execute a aplicação
streamlit run src/app.py

```
Destaque Técnico: Conexão com a Base de Conhecimento
"Diferente de chatbots convencionais, esta aplicação não depende apenas do conhecimento prévio do modelo. 
Antes de cada resposta, o sistema realiza um 'lookup' nos arquivos da pasta `/data`, garantindo que se o valor de uma transferência mudar no CSV, 
a resposta da IA será atualizada automaticamente sem necessidade de retreinamento."

📁 **Pasta:** [`src/`](./src/)

---

### 5. Avaliação e Métricas

A qualidade do AgentBot Analytics é monitorada através de KPIs (Key Performance Indicators) técnicos, garantindo que ele atue como uma fonte da verdade para analistas esportivos.

**Métricas Sugeridas:**
- Precisão e Assertividade Estatística:
O que avalia: A capacidade do agente de extrair o número exato de gols, assistências e valores de mercado contidos nos arquivos data/.
Meta: 100% de fidelidade aos dados estruturados. Qualquer divergência entre o CSV e a resposta da IA é considerada uma falha crítica.
- Taxa de Respostas Seguras (Zero Alucinação):
O que avalia: A eficácia dos filtros de "Anti-Alucinação". Quando perguntado sobre um dado inexistente (ex: "Quantos gols Neymar fez no Real Madrid?"), o agente deve admitir que não possui a informação em vez de inventar um cenário.
Meta: Redução total de respostas inventadas através da técnica de Context Injection.
- Coerência com o Escopo Profissional:
O que avalia: Se o agente mantém a persona de Analista Técnico mesmo sob pressão ou perguntas ambíguas.
Meta: Bloqueio de 100% de perguntas sobre vida pessoal e fofocas, mantendo o tom de voz consultivo e profissional definido no prompt.
- Latência de Recuperação (RAG Efficiency):
O que avalia: O tempo entre a pergunta do usuário e a leitura dos arquivos JSON/CSV para gerar a resposta.
Meta: Respostas em tempo real (menos de 3 segundos) para consultas em bases de dados locais.

Metodologia de Teste
Para validar essas métricas, o protótipo passou por testes de "Stress de Dados":
Teste de Fato: Pergunta-se um dado específico do transacoes.csv e compara-se a resposta com a célula da planilha.
Teste de Provocação: Realizam-se perguntas sobre a vida privada do atleta para validar se o agente mantém os Guardrails de segurança e privacidade.
Teste de Consistência: A mesma pergunta é feita três vezes para garantir que o LLM não mude os números entre uma interação e outra.

📄 **Template:** [`docs/04-metricas.md`](./docs/04-metricas.md)

---

### 6. Pitch

Grave um **pitch de 3 minutos** (estilo elevador) apresentando:

- Qual problema seu agente resolve?
"No jornalismo esportivo e na análise de desempenho, enfrentamos um grande inimigo: a desinformação. Ao buscar estatísticas precisas sobre a carreira de um ícone como Neymar Jr.,
encontramos dados espalhados, fontes contraditórias e IAs genéricas que 'alucinam' números, confundindo gols de amistosos com jogos oficiais ou errando valores de transferências. P
ara um analista ou jornalista,um dado errado é uma perda de credibilidade. A dor que resolvemos é a falta de uma única fonte da verdade técnica."

- A Solução - "O Agente Especialista"
"Para resolver isso, desenvolvi o AgentBot Analytics. Ele não é apenas um chatbot, mas um Agente Inteligente alimentado por uma base de dados estruturada em JSON e CSV. Utilizando a arquitetura RAG (Retrieval-Augmented Generation), o agente não 'tenta lembrar' a informação; ele a consulta em tempo real em nosso Data Lake de performance, saúde e mercado. Ele foi desenhado para ser um consultor técnico que diferencia performance pura de ruídos midiáticos."

- Como ele funciona na prática?
"Na prática, o AgentBot opera sob regras rígidas de segurança e precisão.
Se o usuário pergunta sobre a eficiência de gols no Santos, o bot consulta o arquivo `Carreira e Lesões.json` e entrega o cálculo exato.
Se questionado sobre valores de mercado, ele cruza as informações do `transacoes.csv`.
Além disso, implementamos filtros de anti-alucinação e privacidade: o bot é programado para ignorar fofocas ou vida pessoal, mantendo 100% de foco em métricas profissionais,
garantindo que o usuário receba apenas inteligência de alta qualidade."

- Por que essa solução é inovadora?
"O que torna essa solução inovadora é a transformação de dados brutos em narrativa consultiva segura. Estamos democratizando o acesso ao Scouting de Elite.
O impacto é claro: profissionais tomam decisões mais rápidas, jornalistas entregam fatos mais precisos e eliminamos a desinformação no esporte através da IA Generativa aplicada com responsabilidade técnica."

📄 **Template:** [`docs/05-pitch.md`](./docs/05-pitch.md)

---

## Ferramentas Sugeridas

Todas as ferramentas abaixo possuem versões gratuitas:

| Categoria | Ferramentas |
|-----------|-------------|
| **LLMs** | Gemini](https://gemini.google.com/), [Ollama](https://ollama.ai/) |
| **Desenvolvimento** | [Streamlit](https://streamlit.io/), |
| **Orquestração** | [LangChain](https://www.langchain.com/) |
| **Diagramas** | [Mermaid](https://mermaid.js.org/)|

---

## Estrutura do Repositório

```
📁 agentbot-analytics-neymar/
│
├── 📄 README.md                      # Visão geral e guia de execução
│
├── 📁 data/                          # O "Data Lake" do Agente (Fonte da Verdade)
│   ├── transacoes.csv                # Histórico de transferências e valores de mercado
│   ├── historico_atendimento.csv     # Registro de marcos contratuais e decisões de carreira
│   ├── Carreira e Lesões.json        # Base estatística de performance e histórico médico
│   └── Prêmio e Títulos.json         # Perfil de conquistas e fundamentos técnicos
│
├── 📁 docs/                          # Documentação Técnica Completa
│   ├── 01-documentacao-agente.md     # Caso de uso, Persona e Arquitetura RAG
│   ├── 02-base-conhecimento.md       # Estratégia de mapeamento e tratamento de dados
│   ├── 03-prompts.md                 # Engenharia de Prompts e Guardrails de segurança
│   ├── 04-metricas.md                # Avaliação de assertividade e anti-alucinação
│   └── 05-pitch.md                   # Roteiro e estratégia de apresentação
│
├── 📁 src/                           # Código-fonte da Aplicação
│   └── app.py                        # Protótipo interativo em Streamlit + Python
│
├── 📁 assets/                        # Identidade Visual
│   └── diagramas-arquitetura.png     # Desenho do fluxo de dados
│
└── 📁 examples/                      # Referências de implementação
    └── prompts-testados.md
```

---

## 💡 Dicas Finais para o Projeto
- A Base é o Prompt: Dedique tempo ao System Prompt. É ele quem garante que o agente se comporte como um Analista de Performance e não como um torcedor ou chatbot de fofocas.
- Fidelidade aos Dados: Utilize os arquivos na pasta data/ como a "Única Fonte da Verdade". No esporte de alto nível, uma estatística errada pode invalidar toda uma análise técnica.
- Segurança e Rigor (Anti-Alucinação): Assim como no setor financeiro, evitar alucinações aqui é crítico. Se o dado do gol ou da lesão não está no JSON/CSV, o agente deve ser honesto e reportar a ausência da informação.
- Simule Consultas de Scouting: Teste o agente com perguntas que um Diretor Esportivo ou um Jornalista de Dados faria, como: "Qual a correlação entre as lesões de 2019 e a queda de média de gols?"
- Pitch Objetivo: Você tem 3 minutos. Foque em como a IA resolve o caos da desinformação esportiva através da arquitetura RAG. Menos "história do futebol" e mais "solução tecnológica".
