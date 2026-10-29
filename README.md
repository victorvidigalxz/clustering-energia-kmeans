# Checkpoint 03 — Clustering Aplicado à Energia

**Disciplina:** SERS — Prof. André Tritiack

## Integrante

| Nome | RM |
| --- | --- |
| Victor Vidigal | 571318 |

## Objetivo da atividade

Aplicar o algoritmo **K-Means** para identificar **perfis de consumo de energia** em uma base simulada de unidades consumidoras. A atividade pede para:

1. explorar os dados e padronizar as variáveis;
2. comparar diferentes quantidades de clusters (k) com o **método Elbow** e o **Silhouette Score**;
3. justificar a escolha de k;
4. apresentar gráficos e interpretar os perfis encontrados;
5. propor ações de gestão de energia para cada perfil e discutir as limitações da análise.

## Descrição da base de dados

A base tem **120 unidades consumidoras fictícias**, geradas previamente com NumPy (semente 303) e incorporadas ao próprio notebook como texto CSV. **Não são dados reais** de medidores, empresas ou bases públicas. Não há valores ausentes nem identificadores repetidos.

| Coluna | Significado | Unidade |
| --- | --- | --- |
| `id_unidade` | Identificador da unidade (não entra no modelo) | — |
| `consumo_mensal_kwh` | Energia total consumida no mês | kWh |
| `demanda_maxima_kw` | Maior demanda média em janela de 15 minutos no mês | kW |
| `consumo_noturno_pct` | Parcela do consumo mensal entre 18h e 6h | % |

O agrupamento usa as **três variáveis numéricas**, padronizadas com `StandardScaler`.

## Conteúdo do repositório

| Arquivo | Descrição |
| --- | --- |
| `Checkpoint_3_Clustering_Energia.ipynb` | Notebook preenchido e executado (códigos, tabelas, gráficos, análises e conclusões) |
| `README.md` | Este arquivo |
| `requirements.txt` | Bibliotecas necessárias |

## Como executar o notebook

1. Tenha o Python 3.9 ou superior instalado.
2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
3. Abra o notebook no Jupyter, VS Code ou Google Colab:
   ```bash
   jupyter notebook Checkpoint_3_Clustering_Energia.ipynb
   ```
4. Execute todas as células **na ordem** (*Kernel → Restart & Run All*).

Não é preciso baixar nenhum arquivo externo: a base está dentro do notebook. Os resultados são reproduzíveis porque todos os ajustes do K-Means usam `random_state=42` e `n_init=10`.

## Principais resultados

**Escolha de k.** O Elbow e o Silhouette Score apontam para o mesmo valor: **k = 3**.

| k | Inércia | Silhouette Score |
| ---: | ---: | ---: |
| 1 | 360,00 | — |
| 2 | 184,70 | 0,4835 |
| **3** | **74,25** | **0,6031** |
| 4 | 61,10 | 0,5190 |
| 5 | 52,06 | 0,4282 |
| 6 | 46,36 | 0,4343 |
| 7 | 40,90 | 0,3240 |
| 8 | 37,25 | 0,3106 |

- No Elbow, a queda de inércia é grande até k = 3 (de 110,44 entre k = 2 e k = 3) e passa a ser pequena depois (13,15 entre k = 3 e k = 4).
- O maior Silhouette Score do intervalo testado é o de k = 3 (**0,6031**).

**Perfis encontrados** (modelo final com k = 3, 40 unidades em cada grupo; clusters numerados por consumo médio crescente):

| Cluster | Unidades | Consumo médio (kWh) | Demanda média (kW) | Consumo noturno médio (%) | Perfil | Ação proposta |
| ---: | ---: | ---: | ---: | ---: | --- | --- |
| 0 | 40 | 245,09 | 2,58 | 60,82 | Baixo consumo e baixa demanda | Acompanhamento leve e campanhas de uso consciente |
| 1 | 40 | 876,50 | 6,87 | 25,49 | Consumo médio, uso diurno e picos altos de demanda | Gestão de demanda e estudo de geração solar |
| 2 | 40 | 1433,93 | 6,15 | 67,37 | Alto consumo concentrado à noite | Auditoria energética das cargas noturnas |

**Limitações.** A base é simulada e cobre um único mês, com apenas três variáveis. Consumo alto não prova desperdício, e recomendações sobre tarifa ou geração solar exigiriam dados reais de contrato, curva de carga horária, localização e viabilidade. O K-Means exige definir k, é sensível a escala e a valores atípicos e atribui cada unidade a um único grupo. A análise completa está no notebook.
