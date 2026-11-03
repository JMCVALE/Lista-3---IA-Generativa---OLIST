---
name: skill-analytics
description: "Skill que responde consultas de análise de vendas usando os dados da Olist. Disponibiliza total de vendas por estado, ranking de produtos, tempo médio de entrega, top 5 consumidos gerais e top 10 vendedores, com tabelas."
---

# Skill de Análise de Vendas Olist

## 1. Tarefa e resultado esperado

Realizar análises de vendas a partir dos arquivos CSV da pasta **olist** e apresentar respostas às seguintes perguntas:
1. Total de vendas por estado.
2. Ranking dos produtos mais vendidos.
3. Tempo médio de entrega.
4. Os 5 maiores itens consumidos (quantidade total).
5. Os 10 maiores vendedores (valor vendido).

- **Objetivo:** permitir que o usuário obtenha rapidamente esses insights sem escrever código.
- **Incluído:** leitura dos CSVs, cálculo dos indicadores, geração de tabelas.
- **Fora do escopo:** análises que exigem outras fontes de dados, predição ou modelagem avançada.

## 2. Quando usar

Use esta skill quando for necessário analisar as vendas da Olist para um **período** específico, filtrando por **estado**, **cliente** ou **vendedor**. Exemplo de pedido:
```
Quero o total de vendas por estado para o período 2022-01-01 a 2022-12-31, considerando apenas o estado SP.
```

## 3. Entradas

| Entrada | Formato ou origem | Obrigatória | Como usar ou validar |
|---|---|---|---|
| período | `YYYY-MM-DD` a `YYYY-MM-DD` (string) | Sim | Verificar se a data inicial ≤ data final. |
| estado | Código do estado (ex.: `SP`, `RJ`) ou `*` para todos | Não | Caso informado, filtra pedidos pelo campo `estado` na tabela `cliente.csv`. |
| filtroCliente | ID do cliente ou `*` | Não | Se fornecido, seleciona apenas pedidos desse cliente. |
| filtroVendedor | ID do vendedor ou `*` | Não | Se fornecido, seleciona apenas pedidos desse vendedor. |

Quando faltar uma entrada obrigatória, a skill pergunta ao usuário. Para entradas inválidas, a skill informa o erro e solicita correção.

## 4. Saída

- **Entrega:** relatório em texto contendo os cinco indicadores solicitados.
- **Formato:** texto + tabelas.
- **Destino:** a skill devolve o texto no chat.
- **Evidências:** script Python executado (`scripts/analysis.py`) e logs de execução.

## 5. Regras específicas da tarefa

1. Todos os cálculos são baseados nos arquivos CSV da pasta `olist` e respeitam os filtros de entrada.
2. Valores monetários são apresentados em reais (R$) com duas casas decimais.
3. Se o período solicitado não possuir dados, a skill informa "sem registros para o intervalo especificado".

## 6. Procedimento

### Compreender o pedido
1. Identificar o objetivo (qual dos 5 indicadores foram solicitados).
2. Verificar as entradas fornecidas (período, estado, filtros).
3. Caso alguma entrada obrigatória falte ou esteja inválida, solicitar ao usuário.

### Executar a tarefa
4. Invocar o script `scripts/analysis.py` passando os parâmetros via linha de comando.
5. O script lê os CSVs, aplica os filtros e gera os indicadores.
6. Capturar a saída JSON do script e formatar a resposta textual.

### Conferir o resultado
7. Comparar o número de linhas processadas com o total esperado (log do script).
8. Verificar se os arquivos de imagem foram criados com sucesso.
9. Caso haja divergência, relatar ao usuário e sugerir re‑execução.

### Entregar
10. Apresentar o relatório textual e fornecer links para os PNGs gerados.
11. Informar limitações (ex.: dados incompletos, período fora do alcance).

## 7. Recursos de apoio

- **Referências:** `olist/` contém os CSVs de origem.
- **Scripts:** `scripts/analysis.py` – script Python que realiza os cálculos.

## 8. Ambiente e ferramentas

- **Ferramentas necessárias:** Python 3.9+, biblioteca `pandas`.
- **Dependências:** listadas em `requirements.txt`.
- **Execução:** `python scripts/analysis.py --periodo "2022-01-01:2022-12-31" --estado SP --cliente * --vendedor *`

## 9. Limites de atuação

- **Preservar:** arquivos CSV originais na pasta `olist`.
- **Alterações previstas:** apenas criação de arquivos em `scripts/`.
- **Decisões a esclarecer:** formato de data aceita (ISO 8601) e unidades monetárias.

## 10. Critérios de conclusão

- Relatório contém os cinco indicadores solicitados.
- Tabelas correspondentes foram gerados e disponibilizados.
- Usuário confirma que as respostas atendem ao pedido.

## 11. Exemplos de uso

### Pedido pertinente
> Quero o ranking dos produtos mais vendidos e o tempo médio de entrega para o período 2023-01-01 a 2023-06-30, filtrando apenas o estado MG.

**Resultado esperado:** tabela de ranking, valor médio de dias de entrega, gráficos de barras.

### Entrada ausente ou inválida
> Quero o total de vendas por estado, mas não informei o período.

**Comportamento esperado:** a skill solicita o período antes de prosseguir.

### Pedido fora do escopo
> Gere um modelo de machine‑learning para prever churn.

**Comportamento esperado:** a skill indica que a solicitação está fora do escopo da skill de análise de vendas.

---

**Arquivos auxiliares** (gerados na mesma pasta):
- `requirements.txt`
- `scripts/analysis.py`
