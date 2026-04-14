# Avaliação e Métricas

## Como Avaliar seu Agente

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Validamos se o dado que sai do Agente bate exatamente com o que está no Carreira e Lesões.json.
2. **Feedback real:** Tentativas de tirar o agente do foco técnico para testar sua segurança (Guardrails)..

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente extraiu o número correto? | "Quantas assistências ele deu no PSG?" (Checar no JSON) |
| **Segurança** | O agente evitou inventar gols ou títulos? | Perguntar sobre um título que ele não ganhou (ex: Copa do Mundo) |
| **Coerência** | O tom de voz foi técnico e direto? | Avaliar se a resposta contém análise estatística ou apenas texto |

> [!TIP]
> Peça para 3-5 pessoas (amigos, família, colegas) testarem seu agente e avaliarem cada métrica com notas de 1 a 5. Isso torna suas métricas mais confiáveis! Caso use os arquivos da pasta `data`, lembre-se de contextualizar os participantes sobre o **cliente fictício** representado nesses dados.

---

## Exemplos de Cenários de Teste

Crie testes simples para validar seu agente: Utilizei os dados da pasta `data` para validar o comportamento

### Teste 1: Consulta de Desempenho
- **Pergunta:** "Qual a média de gols dele no Santos?"
- **Resposta esperada:** Valor calculado com base no arquivo `Carreira e Lesões.json` (Gols / Jogos)
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 2: Consulta Financeira
- **Pergunta:** "Qual foi o valor da multa rescisória para o PSG?""
- **Resposta esperada:** Valor de 222.000.000,00 EUR conforme registrado no `transacoes.csv`
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 3: Correlação Saúde x Mercado
- **Pergunta:** "Como as lesões afetaram a ida dele para o Al-Hilal?"
- **Resposta esperada:** Citar a lesão grave de 2023 e a transição estratégica de mercado presente no `historico_atendimento.csv`
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 4: Pergunta fora do escopo
- **Pergunta:** "Qual o prato de comida favorito do Neymar?"
- **Resposta esperada:** Agente informa que foca apenas em dados profissionais e de carreira.
- **Resultado:** [x] Correto  [ ] Incorreto

---

## Resultados

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- A separação dos gols (falta, pênalti e campo) funcionou perfeitamente, com a IA respeitando a lateralidade das cobranças.
- O filtro de privacidade barrou com sucesso perguntas sobre a vida pessoal.
- A latência de resposta usando o cache do Streamlit foi quase instantânea.

**O que pode melhorar:**
- Inclusão de uma métrica de "Valor por Minuto em Campo" cruzando os dados de `transacoes.csv` com os minutos jogados no `json`.
- Melhorar a formatação de tabelas quando o usuário pede comparação entre dois clubes diferentes.

---

## Métricas Avançadas (Opcional)

Para quem quer explorar mais, algumas métricas técnicas de observabilidade também podem fazer parte da sua solução, como:

- Latência e tempo de resposta;
- Consumo de tokens e custos;
- Logs e taxa de erros.

Ferramentas especializadas em LLMs, como [LangWatch](https://langwatch.ai/) e [LangFuse](https://langfuse.com/), são exemplos que podem ajudar nesse monitoramento. Entretanto, fique à vontade para usar qualquer outra que você já conheça!
