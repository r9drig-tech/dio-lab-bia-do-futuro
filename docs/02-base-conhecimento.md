# Base de Conhecimento


> [!TIP]
> **Prompt usando para etapa:**
> 
> Preciso organizar a base de conhecimento do meu agente analítico da carreira do Neymar Jr.
> Tenho estes arquivos de dados: [liste os arquivos],
> Ajude me:
> (1)entender o que cada arquivo contém,
> (2) decidir como analisar cada um,
> (3) criar um exemplo de contexto formatado para incluir no prompt.


## Dados Utilizados


| Arquivo | Formato | Pra que serve AgentBot |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV |Registra o log cronológico de negociações, renovações e propostas recusadas |
| `Carreira e Lesões.json` | JSON | Contém a volumetria técnica: gols (falta, pênalti, campo), assistências e histórico médico |
| `Prêmio e Títulos.json` | JSON | Dados de Premios Individais e Titulos por clubes |
| `transacoes.csv` | CSV | Analisa a evolução financeira: salários por período, multas rescisórias e valor de mercado |

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

Existem duas possibilidades principais para alimentar o agente: a injeção manual de dados diretamente no prompt ou o carregamento automatizado via código. Para este projeto, optamos pela segunda abordagem para garantir escalabilidade e precisão.

O sistema utiliza Python para carregar as informações. Abaixo, demonstro a implementação técnica utilizada para ler as bases CSV e JSON de forma estruturada:
````

import pandas as pd
import json

# Carregamento de arquivos CSV (Transações e Histórico de Marcos)
historico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

# Carregamento de arquivos JSON (Estatísticas de Carreira e Títulos)
with open('data/Carreira e Lesões.json', 'r', encoding='utf-8') as f:
    carreira_lesoes = json.load(f)

with open('data/Prêmio e Títulos.json', 'r', encoding='utf-8') as f:
    premios_titulos = json.load(f)

````
### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?
Para garantir o melhor contexto possível, os dados são injetados no prompt. Em soluções robustas, essa carga é feita dinamicamente para ganhar flexibilidade e economizar tokens.
Abaixo, apresento a estrutura real dos dados utilizados:
 
````

DADOS DA CARREIRA: (Carreira e Lesões.json)
[
  {
    "clube": "Santos FC",
    "periodo": "2009 - 2013",
    "tecnico": {
      "gol_carreira": 138,
      "ast_carreira": 65,
      "faltas": 5,
      "penalti": 22,
      "perna_dir": 110,
      "perna_esq": 18,
      "outros_gols": 10,
      "dribles_certos": "82%",
      "passes_certos": "79%"
    },
    "medico": {
      "lesoes": 4,
      "afastado": "62 dias",
      "historico": [
        "- Pelve (2010 - 5 dias)",
        "- Tornozelo (2011 - 15 dias)",
        "- Lesão Muscular (2012 - 10 dias)",
        "- Tornozelo (2013 - 37 dias)"
      ]
    },
    "jogos": { "titular": 220, "reserva": 10, "amarelos": 58, "vermelhos": 3 },
    "recordes_e_marcos": [
      "- Vencedor do FIFA Puskás Award 2011 (Gol contra o Flamengo)",
      "- Protagonista do 'Duelo de Gerações' contra Ronaldinho Gaúcho (Santos 4x5 Flamengo)",
      "- Maior artilheiro do Santos na era pós-Pelé (138 gols)",
      "- Gol do título da Libertadores 2011 após 48 anos de jejum"
    ]
  },
  {
    "clube": "FC Barcelona",
    "periodo": "2013 - 2017",
    "tecnico": {
      "gol_carreira": 105,
      "ast_carreira": 76,
      "faltas": 2,
      "penalti": 15,
      "perna_dir": 82,
      "perna_esq": 15,
      "outros_gols": 8,
      "dribles_certos": "88%",
      "passes_certos": "84%"
    },
    "medico": {
      "lesoes": 8,
      "afastado": "145 dias",
      "historico": [
        "- Tornozelo (25 dias)",
        "- Edema no Pé (20 dias)",
        "- Vértebra (30 dias)",
        "- Caxumba (15 dias)",
        "- Metatarso (31 dias)"
      ]
    },
    "jogos": { "titular": 175, "reserva": 11, "amarelos": 42, "vermelhos": 1 },
    "recordes_e_marcos": [
      "- Protagonista da 'Remontada' 6-1 contra o PSG, Champions 2016-17",
      "- Primeiro jogador a marcar em todas as fases de mata-mata (Quartas, Semis e Final) da Champions 2014-15",
      "- Integrante do Trio MSN - Recorde de 131 gols em uma única temporada 2015-16",      
      "- Único jogador a marcar em finais de Copa do Rei e Champions League na mesma temporada (2014-15)",
    ]
  },
  {
    "clube": "Paris Saint-Germain",
    "periodo": "2017 - 2023",
    "tecnico": {
      "gol_carreira": 118,
      "ast_carreira": 77,
      "faltas": 9,
      "penalti": 30,
      "perna_dir": 95,
      "perna_esq": 20,
      "outros_gols": 3,
      "dribles_certos": "91%",
      "passes_certos": "82%"
    },
    "medico": {
      "lesoes": 22,
      "afastado": "720 dias",
      "historico": [
        "- Metatarso Direito (2018 - 90 dias)",
        "- Metatarso Direito (2019 - 85 dias)",
        "- Tornozelo Direito - Cirurgia (2023 - 130 dias)",
        "- Outras 19 ocorrências (Musculares/Pancadas)"
      ]
    },
    "jogos": { "titular": 165, "reserva": 8, "amarelos": 48, "vermelhos": 4 },
    "recordes_e_marcos": [
      "- Transferência mais cara da história (€222M)",
      "- Levou o PSG à sua primeira final de Champions League 2019-20",
      "- Maior média de dribles da história da Ligue 1 (5.2 p/j)"
    ]
  },
  {
    "clube": "Al-Hilal",
    "periodo": "2023 - 2025",
    "tecnico": {
      "gol_carreira": 1,
      "ast_carreira": 3,
      "faltas": 0,
      "penalti": 0,
      "perna_dir": 1,
      "perna_esq": 0,
      "dribles_certos": "75%",
      "passes_certos": "88%"
    },
    "medico": {
      "lesoes": 2,
      "afastado": "370 dias",
      "historico": [
        "- Lesão Muscular (20 dias)",
        "- Ruptura Ligamento ACL (350 dias)"
      ]
    },
    "jogos": { "titular": 5, "reserva": 0, "amarelos": 1, "vermelhos": 0 }
  },
  {
    "clube": "Santos FC",
    "periodo": "2025 - Atual",
    "tecnico": {
      "gol_carreira": 12,
      "ast_carreira": 8,
      "faltas": 1,
      "penalti": 3,
      "perna_dir": 10,
      "perna_esq": 2,
      "dribles_certos": "86%",
      "passes_certos": "85%"
    },
    "medico": {
      "lesoes": 1,
      "afastado": "15 dias",
      "historico": ["- Pancada Muscular (15 dias)"]
    },
    "jogos": { "titular": 18, "reserva": 2, "amarelos": 4, "vermelhos": 0 },
  },
  {
    "clube": "Seleção Brasileira Sub-17",
    "periodo": "2009",
    "tecnico": { "gol_carreira": 1, "ast_carreira": 0, "faltas": 0, "penalti": 0, "perna_dir": 1, "perna_esq": 0, "dribles_certos": "80%", "passes_certos": "75%" },
    "medico": { "lesoes": 0, "afastado": "0 dias", "historico": ["- Sem lesões"] },
    "jogos": { "titular": 3, "reserva": 0, "amarelos": 0, "vermelhos": 0 },
    "recordes_e_marcos": [
      "- Artilheiro da Copa Sendai 2009 (1 Gol)",
    ]
  },
  {
    "clube": "Seleção Brasileira Sub-20",
    "periodo": "2011",
    "tecnico": { "gol_carreira": 9, "ast_carreira": 2, "faltas": 1, "penalti": 2, "perna_dir": 7, "perna_esq": 2, "dribles_certos": "85%", "passes_certos": "78%" },
    "medico": { "lesoes": 0, "afastado": "0 dias", "historico": ["- Sem lesões"] },
    "jogos": { "titular": 7, "reserva": 0, "amarelos": 1, "vermelhos": 0 },
    "recordes_e_marcos": [
      "- Protagonista do título Sul-Americano com 9 gols em 7 jogos",
      "- Artilheiro isolado do Sul-Americano Sub-20 2011"
    ]
  },
  {
    "clube": "Seleção Brasileira Sub-23",
    "periodo": "2012 - 2016",
    "tecnico": { "gol_carreira": 11, "ast_carreira": 6, "faltas": 1, "penalti": 2, "perna_dir": 8, "perna_esq": 3, "dribles_certos": "88%", "passes_certos": "82%" },
    "medico": { "lesoes": 0, "afastado": "0 dias", "historico": ["- Sem lesões"] },
    "jogos": { "titular": 12, "reserva": 0, "amarelos": 2, "vermelhos": 0 },
    "recordes_e_marcos": [
      "- Marcou o gol do 1º Ouro Olímpico da história (Rio 2016)",
      "- Gol mais rápido da história das Olimpíadas (14 segundos)",
      "- Medalha de Prata (Londres 2012)"
    ]
  },
  {
    "clube": "Seleção Brasileira",
    "periodo": "2010 - Atual",
    "tecnico": {
      "gol_carreira": 100,
      "ast_carreira": 67,
      "faltas": 5,
      "penalti": 28,
      "perna_dir": 81,
      "perna_esq": 15,
      "dribles_certos": "85%",
      "passes_certos": "81%"
    },
    "medico": {
      "lesoes": 5,
      "afastado": "545 dias (total)",
      "historico": [
        "- Fratura Vértebra L3 (Copa 2014)",
        "- Ruptura Ligamento Tornozelo (Copa América 2019)",
        "- Ruptura ACL e Menisco (Eliminatórias 2023)"
      ]
    },
    "recordes_e_marcos": [
      "- Maior artilheiro da história da Seleção (79 gols oficiais)",
      "- Maior Assistencia da história da Seleção (59 Assistência)",
      "- Um dos três brasileiros a marcar em 3 Copas do Mundo diferentes (2014, 2018, 2022)",
      "- Protagonista do Ouro Olímpico Inédito (Rio 2016)",
      "- Artilheiro da Seleção no ciclo de eliminatórias de 2018 e 2022"
    ]
  }
]

TRANSACOES DO NEYMAR:(transacoes.csv)
data,descricao,categoria,valor,tipo
2004-01-01,Auxílio Formação Santos,contrato_base,5000.00,entrada
2006-03-01,Bônus de Permanência (Recusa ao Real Madrid),investimento_base,1000000.00,recusado
2008-01-01,Contrato de Formação Avançado,contrato_base,240000.00,entrada
2009-02-01,Primeiro Contrato Profissional Santos,contrato_profissional,1000000.00,entrada
2010-08-23,Chelsea,transferencia,30000000.00,recusado
2011-06-15,Real Madrid,transferencia,45000000.00,recusado
2011-11-09,Renovação de Contrato Santos,renovacao,2000000.00,entrada
2013-06-01,Saída Santos para Barcelona (Venda),transferencia,88400000.00,saida
2013-07-01,Contrato Inicial Barcelona,contrato_profissional,5000000.00,entrada
2016-06-15,Contrato Manchester United,salario_anual,25000000.00,recusado
2016-07-01,Renovação de Contrato Barcelona,renovacao,9000000.00,entrada
2017-08-03,Saída Barcelona para PSG (Multa Rescisória),transferencia,222000000.00,saida
2017-08-15,Contrato Inicial PSG,contrato_profissional,30000000.00,entrada
2019-08-30,Retorno Barcelona,transferencia,170000000.00,recusado
2021-05-01,Renovação de Contrato PSG,renovacao,36000000.00,entrada
2022-07-01,Contrato Newcastle,transferencia,100000000.00,recusado
2023-08-15,Saída PSG para Al-Hilal (Venda),transferencia,90000000.00,saida
2023-09-01,Contrato Inicial Al-Hilal,contrato_profissional,160000000.00,entrada
2025-01-01,Saída Al-Hilal (Fim de Contrato),transferencia,0.00,saida
2025-01-01,Contrato Inicial Santos,contrato_profissional,1500000.00,entrada

HISTORICO DE ATENDIMENTO:(historico_atendimento.csv
data,categoria,evento,resumo,resolvido,contexto_ia
2006-03-01,MERCADO,Recusa Real Madrid,Jogador recusou contrato aos 14 anos após testes na Espanha,sim,Fidelidade ao Santos para maturação técnica no Brasil.
2010-08-23,NEGOCIACAO,Recusa Chelsea,Santos e Neymar recusam proposta de 30M de euros,sim,Decisão estratégica para conquistar títulos na América antes da Europa.
2011-06-15,NEGOCIACAO,Recusa Real Madrid,Jogador recusa nova investida do clube espanhol,sim,Foco total na disputa do Mundial de Clubes pelo Santos.
2011-11-09,RENOVACAO,Extensão Santos,Renovação para garantir permanência até a Copa de 2014,sim,Consolidação como maior ídolo nacional pós-Pelé.
2013-05-25,TRANSFERENCIA,Ida ao Barcelona,Fechamento de contrato histórico com o clube catalão,sim,Início da fase de auge internacional e parceria com Messi.
2016-07-01,RENOVACAO,Renovação Barcelona,Extensão de contrato após conquista da Champions League,sim,Manutenção do domínio europeu e estabilidade no trio MSN.
2017-08-03,TRANSFERENCIA,Ida ao PSG,Quebra de cláusula recorde de 222M de euros,sim,Busca por protagonismo absoluto e tentativa de ser o melhor do mundo.
2019-08-30,NEGOCIACAO,Tentativa Retorno,PSG recusou propostas de retorno ao Barcelona,sim,Início de desgaste com o projeto francês e busca por ambiente conhecido.
2023-08-15,TRANSFERENCIA,Ida ao Al-Hilal,Aceitou proposta recorde após PSG sinalizar fim de ciclo,sim,Transição para mercado árabe após sequência de lesões na Europa.
2025-01-01,RETORNO,Volta ao Santos,Reunião de acerto para retorno ao clube de origem,sim,Projeto estratégico visando a preparação física para a Copa 2026.

PREMIO E TITULOS:(Prêmio e Títulos.json)
[
  {
    "clube": "Santos FC",
    "periodo": "2009 - 2013",
    "titulos": {
      "internacionais": ["Libertadores (2011)", "Recopa Sul-Americana (2012)"],
      "nacionais": ["Copa do Brasil (2010)","Campeonato Paulista (2010, 2011, 2012)"]
    },
    "premios_individuais": [
      "- FIFA Puskás Award 2011",
      "- Rei da América (2011, 2012)",
      "- Melhor Jogador da Libertadores 2011",
      "- Bola de Bronze Mundial de Clubes 2011 (1 Gol)",
      "- Artilheiro do Campeonato Paulista 2012 (20 Gols)",
      "- Bola de Ouro Copa do Brasil 2010 (11 Gols)",
      "- Melhor Jogador do Campeonato Paulista (2010, 2011, 2012, 2013)"
    ]
  },
  {
    "clube": "FC Barcelona",
    "periodo": "2013 - 2017",
    "titulos": {
      "internacionais": ["UEFA Champions League (2015)", "Mundial de Clubes da FIFA (2015)", "Supercopa da UEFA (2015)"],
      "nacionais": ["La Liga (2014-15, 2015-16)", "Copa del Rey (2014-15, 2015-16, 2016-17)", "Supercopa da Espanha (2013)"]
    },
    "premios_individuais": [
      "- Artilheiro da Champions League 2014-15 (10 Gols)",
      "- Artilheiro da Copa del Rey 2014-15 (7 Gols)",
      "- Samba Gold (2014, 2015)",
      "- Equipe do Ano FIFA/FIFPro 2015",
      "- Melhor Jogador do Mundo (3º lugar Ballon d'Or - 2015)",
      "- Melhor Jogador Estrangeiro da La Liga 2014-15",
      "- Líder de Assistências da Champions League 2016-17 (8 Assistências)"
    ]
  },
  {
    "clube": "Paris Saint-Germain",
    "periodo": "2017 - 2023",
    "titulos": {
      "nacionais": [
        "Ligue 1 (2018, 2019, 2020, 2022, 2023)",
        "Copa da França (2018, 2020, 2021)",
        "Copa da Liga Francesa (2018, 2020)",
        "Supercopa da França (2018, 2020, 2022)"
      ]
    },
    "premios_individuais": [
      "- Melhor Jogador da Ligue 1 2017-18",
      "- Samba Gold (2017, 2020, 2021, 2022)",
      "- Equipe da Temporada da Champions League (2019-20, 2020-21)",
      "- Líder de Assistências da Ligue 1 2017-18 (13 Assistências)",
      "- Equipe do Ano FIFA/FIFPro 2017",
      "- Melhor Jogador do Mundo (3º lugar Ballon d'Or - 2017)"
    ]
  },
  {
    "clube": "Al-Hilal",
    "periodo": "2023 - 2025",
    "titulos": {
      "nacionais": ["Campeonato Saudita (2023-24)", "Supercopa Saudita (2024)", "Copa do Rei Saudita (2024)"]
    },
    "premios_individuais": []
  },
{
    "clube": "Santos FC",
    "periodo": "2025 - Atual",
    "titulos": {
      "nacionais": []
    },
    "premios_individuais": []
  {
    "clube": "Seleção Brasileira Sub-17",
    "periodo": "2009",
    "titulos": {
      "internacionais": ["Copa Sendai (2009)"]
    },
    "premios_individuais": [
      "- Artilheiro da Copa Sendai 2009 (1 Gols)",
    ]
  },
  {
    "clube": "Seleção Brasileira Sub-20",
    "periodo": "2011",
    "titulos": {
      "internacionais": ["Sul-Americano Sub-20 (2011)"]
    },
    "premios_individuais": [
      "- Artilheiro do Sul-Americano Sub-20 2011 (9 Gols)",
      "- Melhor Jogador do Sul-Americano Sub-20 2011"
    ]
  },
  {
    "clube": "Seleção Brasileira Sub-23",
    "periodo": "2012 - 2016",
    "titulos": {
      "internacionais": [
        "Ouro Olímpico (Rio 2016)", 
        "Prata Olímpica (Londres 2012)"]
    },
    "premios_individuais": [
      "- Artilheiro do Brasil nas Rio 2016 (4 Gols)",
      "- Maior Assistente do Brasil nas Rio 2016 (3 Assistências)",
      "- Recorde: Gol mais rápido da história das Olimpíadas (14 segundos contra Honduras)",
      "- Eleito para o Time Ideal (Best XI) das Olimpíadas pela crítica especializada",
      "- Autor do gol de falta na Final e do pênalti decisivo do Ouro"
    ]
  },
  {
    "clube": "Seleção Brasileira",
    "periodo": "2010 - Atual",
    "titulos": {
      "internacionais": [
        "Copa das Confederações 2013", 
        "Superclássico das Américas (2011, 2012, 2014, 2018)"
      ]
    },
    "premios_individuais": [
      "- Maior Artilheiro da História da Seleção Brasileira (79 Gols)",
      "- Bola de Ouro Copa das Confederações 2013",
      "- Chuteira de Bronze Copa das Confederações 2013 (4 Gols)",
      "- 2x Man of the Match em Copas do Mundo 2014 (Croácia/Camarões)",
      "- Man of the Match em Copas do Mundo 2018 (México)",
      "- Man of the Match em Copas do Mundo 2022 (Coreia do Sul)",
      "- Chuteira de Bronze Copa do Mundo 2014 (4 gols e 1 assistência)",
      "- Equipe do Ano da CONMEBOL 2021 (Copa América)"
    ]
  }
]

````
---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo de contexto montado abaixo baseia-se nos dados originais da base de conhecimento, sintetizando-os para manter apenas as informações mais relevantes. 
Isso otimiza o consumo de tokens, garantindo que o agente tenha o contexto necessário para uma resposta precisa.

Nota Técnica: Este é o formato final dos dados após o processamento do Python, pronto para ser injetado no prompt. Ele consolida informações de quatro fontes diferentes (Técnico, Médico, Mercado de Transferência e Status).
        
```
[DADOS TÉCNICOS]
SANTOS FC (2009-2013)
- Performance: 138 Gols | 65 Assistências
- Finalização: Perna Dir: 110 | Perna Esq: 18 | Faltas: 5 | Pênaltis: 22 | Outros: 10
- Eficiência: Dribles Certos: 82% | Passes Certos: 79%
- Disciplina: 220 Jogos Titular | 58 Cartões Amarelos | 3 Vermelhos
- Conquistas:
      Copa Libertadores (2011)
      Copa do Brasil (2010)
      Recopa Sul-Americana (2012)
      Paulista (2010, 2011, 2012)
- Prêmios Individuais:
      FIFA Puskás Award (2011)
      Rei da América (2011, 2012)
      Artilheiro do Campeonato Paulista 2012 (20 gols)
- Recordes e marcos:
      Vencedor do FIFA Puskás Award 2011 (Gol contra o Flamengo)
      Protagonista do 'Duelo de Gerações' contra Ronaldinho Gaúcho (Santos 4x5 Flamengo)
      Maior artilheiro do Santos na era pós-Pelé (138 gols)
      Gol do título da Libertadores 2011 após 48 anos de jejum

> [HISTÓRICO MÉDICO DETALHADO]
- Total de Lesões no Período: 4 registros
- Tempo Total Afastado: 62 dias
- Eventos Notáveis:
      Pelve (2010): 5 dias
      Tornozelo (2011): 15 dias
      Lesão Muscular (2012): 10 dias
      Tornozelo (2013): 32 dias

[HISTÓRICO MERCADO DE TRANSFERENCIA]
- Evento de Negociação: Recusa ao Real Madrid (2011).
- Valor de Mercado na Época: 45M Euros.
- Justificativa: Essa decisão permitiu a conquista de títulos continentais antes de sua transferência para o FC Barcelona em 2013.

[STATUS ATUAL]
- Atividade: Titular absoluto e capitão, atuando em alta intensidade.
- Condição Física: 100% (Livre de lesões e restrições médicas).
- Foco Estratégico: Preparação integral para a Copa do Mundo 2026.
- Situação Contratual: Estabilizada, priorizando ritmo de jogo e performance técnica.
```
