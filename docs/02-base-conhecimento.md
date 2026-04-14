# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV |Registra o log cronológico de negociações, renovações e propostas recusadas |
| `Carreira e Lesões.json` | JSON | Contém a volumetria técnica: gols (falta, pênalti, campo), assistências e histórico médico |
| `Prêmio e Títulos.json` | JSON | Dados de Premios Individais e Titulos por clubes |
| `transacoes.csv` | CSV | Analisa a evolução financeira: salários por período, multas rescisórias e valor de mercado |

> [!TIP]
> **Diferencial Estratégico**: Essa separação permite que o Agente diferencie valor de mercado (transacoes.csv) de eficiência técnica (carreira.json),
evitando confusão entre custos de transferência e desempenho em campo

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados originais foram expandidos para garantir que o AgentBot atue como um analista de elite:
- Mapeamento de Faltas: Inclusão da lateralidade dos gols de falta (setores esquerdo, central e direito) para análise de precisão.
- Log de Decisões: Registro de propostas negadas (Real Madrid, Newcastle) para contextualizar a estratégia de carreira "Brasil -> Europa -> Ásia".
- Métrica de Saúde: Inclusão de um histórico de lesões correlacionado com o tempo de inatividade, explicando a transição de valores entre PSG e Al-Hilal.
- Retroatividade: Dados financeiros e contratuais consolidados desde o primeiro contrato profissional em 2009

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

O sistema utiliza a biblioteca Pandas para a manipulação dos CSVs e o módulo JSON para os arquivos de performance. 
No ambiente Streamlit, esses dados são carregados em cache para garantir que a consulta aos marcos da carreira seja instantânea

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

O agente utiliza Context Injection baseado em HTTP (simulado via requisições locais ou API). 
Ao receber uma pergunta, o AgentBot busca nos arquivos apenas as "fatias" de dados relevantes (ex: apenas a linha do Santos se a pergunta for sobre 2011) e as injeta no prompt como fatos imutáveis. 
Isso impede que o modelo "alucine" números de gols ou valores de salários.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
[DADOS TÉCNICOS: SANTOS FC (2009-2013)]
- Performance: 138 Gols | 65 Assistências
- Finalização: Perna Dir: 110 | Perna Esq: 18 | Faltas: 5 | Pênaltis: 22
- Eficiência: Dribles Certos: 82% | Passes Certos: 79%
- Disciplina: 220 Jogos Titular | 58 Cartões Amarelos | 3 Vermelhos
- Conquistas Coletivas: Copa Libertadores (2011)
                        Copa do Brasil (2010)
                        Recopa Sul-Americana (2012)
                        Tricampeonato Paulista (2010, 2011, 2012)
- Prêmios Individuais:  FIFA Puskás Award (2011)
                        Rei da América (2011, 2012)
                        Artilheiro do Campeonato Paulista (2012 - 20 gols)

> [HISTÓRICO MÉDICO DETALHADO]
- Total de Lesões no Período: 4 registros
- Tempo Total Afastado: 62 dias
- Eventos Notáveis: Pelve (2010): 5 dias
                    Tornozelo (2011): 15 dias
                    Lesão Muscular (2012): 10 dias
                    Tornozelo (2013): 32 dias

[HISTÓRICO FINANCEIRO / MERCADO]
- Evento de Negociação: Recusa ao Real Madrid (2011).
- Valor de Mercado na Época: 45M Euros.
- Justificativa: Extensão contratual para foco no projeto nacional (Libertadores/Mundial).

[STATUS ATUAL]
- Atividade: Titular absoluto e capitão, atuando em alta intensidade.
- Condição Física: 100% (Livre de lesões e restrições médicas).
- Foco Estratégico: Preparação integral para a Copa do Mundo 2026.
- Situação Contratual: Estabilizada, priorizando ritmo de jogo e performance técnica.
...
```
