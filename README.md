# 🤖 AgentBot Analytics — Neymar Jr. Career Consultant

> O AgentBot Analytics é um agente de inteligência de dados especializado na trajetória profissional de Neymar Jr. Desenvolvido como projeto final para o Bootcamp GenIA e Dados da DIO, ele combina o poder da IA Generativa com uma curadoria rigorosa de dados estatísticos, médicos e contratuais.

## 📌 Visão Geral

>Diferente de assistentes genéricos, este agente baseia-se em uma Base de Conhecimento Proprietária estruturada via Python. O foco principal foi a Engenharia do Dado: garantir que métricas de desempenho, histórico de lesões e títulos estivessem limpos e segmentados para evitar alucinações da IA.

## 🏗️ Arquitetura e Estrutura do Repositório

>O projeto foi organizado seguindo as melhores práticas de desenvolvimento, separando a camada de dados da camada de documentação técnica:

## 📁 Estrutura do Projeto

```
_
📁 agentbot-analytics-neymar/
│
├── 📁 data/                          # O "Data Lake" do Agente (Fonte da Verdade)
│   ├── transacoes.csv                # Histórico de transferências e valores de mercado
│   ├── historico_atendimento.csv     # Registro de marcos contratuais e decisões de carreira
│   ├── Carreira e Lesões.json        # Base estatística de performance e histórico médico
│   └── Prêmio e Títulos.json         # Perfil de conquistas e fundamentos técnicos
│
├── 📁 docs/                          # Documentação Técnica Completa
│   ├── 01-documentacao-agente.md     # Persona, Arquitetura RAG e Casos de Uso
│   ├── 02-base-conhecimento.md       # Estratégia de mapeamento e tratamento de dados
│   ├── 03-prompts.md                 # Engenharia de Prompts e Guardrails
│   └── 04-metricas.md                # Avaliação de assertividade (Anti-alucinação)
│
├── 📁 src/                           # Código-fonte da Aplicação
│   └── app.py                        # Protótipo interativo (Python + Streamlit)
│
└── 📁 assets/                        # Identidade Visual e Diagramas

```

## 🛠️ Destaques Técnicos & Implementação
Para entregar um consultor de alta fidelidade, foram aplicadas as seguintes camadas:

Granularidade de Dados Técnicos: Tratamento específico para métricas de passes, cruzamentos, faltas, gols de falta e dribles, organizados por clube.
Segmentação de Títulos: Lógica personalizada para distinguir e contar conquistas de peso como Mundial de Clubes e Libertadores de forma individualizada.
Inteligência Clínica: Mapeamento do histórico médico cruzado com a performance em campo (média de gols/jogos).
RAG (Retrieval-Augmented Generation): O agente consulta as bases JSON/CSV antes de responder, garantindo precisão histórica.

### 1. Instalar Ollama

```bash

Baixar em: ollama.com
ollama pull "llama3"
ollama serve

```

### 2. Instalar Dependências

```bash

pip install streamlit pandas requests
```

### 3. Rodar o Edu

```bash
streamlit run src/app.py
```

