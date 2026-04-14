# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV |Registra o histórico de negociações, reuniões e "atendimentos" da carreira (Renovações e Recusas) |
| `carreira.json` | JSON | Contém os fatos detalhados de cada clube (Gols, Assistências, Títulos)s |
| `premio.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisa a evolução financeira (Salários, Multas Rescisórias e Valor de Mercado) |

> [!TIP]
> **Diferencial Estratégico**: O uso de arquivos separados permite que o Agente de IA diferencie o que é desempenho esportivo (JSON) do que é gestão de negócios (CSV)

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados originais foram expandidos para incluir camadas de profundidade que não costumam aparecer em datasets comuns:
Log de Recusas: Inclusão de propostas negadas (Real Madrid, Chelsea, Newcastle) para mapear o poder de negociação.
Motivação de Trocas: Diferenciação entre saídas por protagonismo (PSG 2017) e saídas por fim de ciclo/lesão (Al-Hilal 2025).
Histórico de Base: Dados financeiros desde 2004 (Santos) para análise de valorização a longo prazo.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos são processados utilizando a biblioteca Pandas (para os CSVs) e o módulo JSON nativo do Python. 
Eles são carregados na memória no início da execução do Dashboard Streamlit e transformados em DataFrames para consulta rápida.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

O agente utiliza uma técnica de Context Injection. Quando o usuário faz uma pergunta, o sistema filtra as linhas relevantes da carreira e as injeta no contexto como uma "Memória de Curto Prazo", 
permitindo que a resposta seja baseada em fatos e não apenas em conhecimento genérico do modelo.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Contexto de Negociações (CSV):
- Data: 2010-08-23 | Tema: Contrato Chelsea | Status: Recusado
- Resumo: Santos e Neymar anunciaram a recusa da proposta de 30M de euros para focar no projeto nacional.

Histórico de Transações:
- Tipo: Saída (Barcelona -> PSG)
- Valor: 222.000.000,00
- Categoria: Transferência (Multa Rescisória)

Perfil Atual:
- Clube: Santos
- Status: Projeto de Recomeço após lesão no Al-Hilal.
...
```
