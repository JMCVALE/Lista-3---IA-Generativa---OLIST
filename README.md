# 📊 Projeto Skill‑Analytics (Olist)

## Visão geral
Este repositório contém uma **skill** de análise de vendas da Olist, desenvolvida para o **Antigravity**.  A skill oferece respostas automáticas a perguntas de negócios a partir dos arquivos CSV originais da Olist, sem a necessidade de escrever código manualmente.

A estrutura principal é:
```
LISTA 3/
│   README.md                 ← este documento
│   requirements.txt          ← dependências Python
│
├── olist/                     ← conjunto de dados originais (CSV)
│   ├─ avaliacao_pedido.csv
│   ├─ cliente.csv
│   ├─ item_pedido.csv
│   ├─ pagamento_pedido.csv
│   ├─ pedido.csv
│   ├─ produto.csv
│   └─ vendedor.csv
│
├── skill-analytics/           ← definição da skill
│   ├─ SKILL.md                ← especificação da skill
│   └─ requirements.txt        ← dependências da skill
│
└── scripts/                  ← scripts Python que realizam os cálculos
    └─ analysis.py            ← lógica central usada pela skill
```

## Funcionalidades da skill
A skill responde às seguintes consultas (conforme o **SKILL.md**):
1. **Total de vendas por estado**
2. **Ranking dos produtos mais vendidos**
3. **Tempo médio de entrega**
4. **Os 5 maiores itens consumidos (quantidade)**
5. **Os 10 maiores vendedores (valor vendido)**
6. **Filtragem por período e/ou estado** (ex.: "total de vendas de MG entre 2017‑07‑01 e 2017‑12‑31").

Cada operação é executada por scripts Python que leem os CSVs, fazem os merges e produzem saídas de texto.

## Dependências
```text
python>=3.9
pandas
```
Instale tudo com:
```bash
pip install -r requirements.txt
```
*(O `requirements.txt` já está na raiz e na pasta `skill-analytics/`.)*

## Como usar
### 1️⃣ Executar uma análise manualmente
Os scripts de exemplo foram gravados em `scratch/` durante as interações.  Para reproduzir qualquer cálculo, basta rodar o script correspondente, por exemplo:
```bash
# Total de vendas por estado
python scripts/analysis.py --task total_por_estado

# Ranking de produtos
python scripts/analysis.py --task ranking_produtos

# Tempo médio de entrega
python scripts/analysis.py --task tempo_medio_entrega
```
Cada script aceita parâmetros `--periodo`, `--estado`, `--cliente` e `--vendedor` (conforme documentação da skill).

### 2️⃣ Usar a skill dentro do Antigravity
1. Abra o **Antigravity IDE** e navegue até a pasta `skill-analytics`.
2. Selecione a skill (`SKILL.md`) e faça a sua pergunta no chat, por exemplo:
   - `Qual o total de vendas por estado?`
   - `Quero o ranking dos produtos mais vendidos.`
   - `Qual o tempo médio de entrega?`
   - `Quais são os 5 maiores itens consumidos?`
   - `Me dê os 10 maiores vendedores.`
   - `Total de vendas de MG entre 2017‑07‑01 e 2017‑12‑31.`
3. A skill invoca `scripts/analysis.py`, gera os resultados e devolve texto + tabelas.

## Resultados já calculados (exemplos)
| Métrica | Resultado |
|---------|-----------|
| **Total vendas por estado** | AC 19 680,62 R$, …, SP 5 998 226,96 R$ |
| **Ranking produtos** (top 10) | 1️⃣ `aca2eb7d…` (527 unid) – *moveis_decoracao* … |
| **Tempo médio de entrega** | **9,33 dias** |
| **Top 5 itens consumidos** | idem ao ranking acima (primeiros 5 IDs) |
| **Top 10 vendedores** | `4869f7a5…` – R$ 229 472,63 … |
| **Vendas MG 2017‑H2** | **R$ 585 794,72** |

## Estrutura dos scripts (ponto de partida)
```python
# scripts/analysis.py (esqueleto simplificado)
import pandas as pd, os, argparse

base = r"c:/Users/julia/Área de Trabalho/LISTA 3/olist"

# Carregamento genérico
pedido = pd.read_csv(os.path.join(base, 'pedido.csv'))
cliente = pd.read_csv(os.path.join(base, 'cliente.csv'))
pagamento = pd.read_csv(os.path.join(base, 'pagamento_pedido.csv'))
item = pd.read_csv(os.path.join(base, 'item_pedido.csv'))
produto = pd.read_csv(os.path.join(base, 'produto.csv'))

# Funções de cada tarefa (total_por_estado, ranking_produtos, ...)
# ... (implementação já existente nos scripts temporários criados durante a sessão)
```

## Como contribuir
1. Fork este repositório.
2. Crie uma branch `feature/<nome>`.
3. Adicione/melhore scripts ou visualizações.
4. Abra um Pull Request descrevendo a mudança.

## Licença
Este projeto usa dados públicos da Olist e está licenciado sob a **MIT License**.
