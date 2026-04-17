# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Registra marcos contratuais, decisões de carreira e contextos de transferências para respostas históricas |
| `Carreira e Lesões.json.json` | JSON | Fonte principal de estatísticas de performance (gols, passes, dribles) cruzadas com o histórico clínico (lesões e tempo de parada). |
| `produtos_financeiros.json` | JSON | Catálogo de conquistas coletivas e individuais, permitindo distinguir títulos oficiais de premiações. |
| `transacoes.csv` | CSV | Contém o histórico financeiro da carreira: valores de transferências, clubes envolvidos e evolução do valor de mercado. |

---

## Adaptações nos Dados
Você modificou ou expandiu os dados mockados?
- Os dados foram refinados para separar Gols de Falta e Cruzamentos como métricas distintas, permitindo uma análise técnica mais profunda.
Além disso, a estrutura de títulos foi corrigida para garantir que conquistas como a Libertadores e o Mundial de Clubes sejam tratadas como entidades únicas e não duplicadas por temporada de transição.

## Estratégia de Integração
Como os dados são carregados?
- Os dados são processados via Python para garantir a tipagem correta (ex: converter valores de transferência para float) antes de serem enviados ao contexto do modelo.

````
import pandas as pd
import json

# Carregamento do Data Lake
estatisticas = json.load(open('./data/Carreira e Lesões.json'))
titulos = json.load(open('./data/Prêmio e Títulos.json'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')

````

## Como os dados são usados no prompt?
Utilizamos a técnica de Injeção de Contexto (Grounding). Os dados são serializados e inseridos no prompt do sistema, 
garantindo que o Agente não invente estatísticas e se baseie apenas nos números reais fornecidos.
````
### ESTATÍSTICAS TÉCNICAS (data/Carreira e Lesões.json)

{
  "clube": "Barcelona",
  "temporadas": "2013-2017",
  "gols": 105,
  "assistencias": 76,
  "gols_falta": 12,
  "lesoes": [
    {"tipo": "Metatarso", "duração": "3 meses", "ano": 2014}
  ]
}

HISTÓRICO DE TRANSFERÊNCIAS (data/transacoes.csv):
data,origem,destino,valor_euro,status
2013-07-01,Santos,Barcelona,88M,concluido
2017-08-03,Barcelona,PSG,222M,concluido
2023-08-15,PSG,Al-Hilal,90M,concluido

````

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.
O sistema sintetiza os dados brutos para economizar tokens, focando no que é relevante para a pergunta do usuário. Exemplo de resumo enviado ao LLM:

```
DADOS CONSOLIDADOS DO ATLETA:
- Clube Atual: Al-Hilal
- Total de Gols na Carreira (Clubes): 370+ 
- Principal Título: UEFA Champions League (2014-15)
- Histórico Clínico Recente: Lesão de LCA (Outubro/2023)

RESUMO DE PERFORMANCE POR CLUBE:
- Santos: 136 gols | 3 títulos principais
- Barcelona: 105 gols | 8 títulos principais
- PSG: 118 gols | 13 títulos principais

TRANSFERÊNCIA RECORDE:
- Barcelona para PSG: €222 milhões (Maior da história)

```
