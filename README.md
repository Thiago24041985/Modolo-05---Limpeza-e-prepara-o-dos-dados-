# 🏥 Módulo 05 - Limpeza e Preparação de Dados (DataSUS / SINASC)

Repositório dedicado ao projeto prático do **Módulo 05** do curso **Profissão: Cientista de Dados (EBAC)**, focado em diagnóstico, tratamento de valores ausentes (*missings*), categorização clínica e padronização de uma base pública de nascidos vivos do **DataSUS** (`SINASC_RO_2019.csv`).

## 💡 Como elaborei este projeto

Desenvolvi o tratamento da base de dados em Jupyter Notebook seguindo uma pipeline estruturada de limpeza (*Data Cleaning*) e preparação para análise exploratória e modelagem:

1. **Auditoria Inicial e Duplicatas:** Carreguei o dataset original com **11.509 registros e 69 variáveis** (`pd.read_csv`) e validei a integridade das linhas utilizando `.drop_duplicates().shape`, confirmando a ausência de registros duplicados.
2. **Diagnóstico de Valores Ausentes (`NaN`):** Realizei um levantamento completo de valores faltantes com `.isna().sum()`, filtrando e ordenando apenas as colunas afetadas e calculando o percentual exato de ausência por variável para identificar campos críticos (como `DTRECORIGA` com 100% de ausência e `CODANOMAL` com ~99,17%).
3. **Seleção de Variáveis (*Feature Selection*):** Reduzi o escopo para as **9 variáveis relevantes** ao estudo de risco perinatal (`LOCNASC`, `IDADEMAE`, `ESTCIVMAE`, `ESCMAE`, `QTDFILVIVO`, `GESTACAO`, `GRAVIDEZ`, `CONSULTAS` e `APGAR5`).
4. **Tratamento da Variável Resposta (`APGAR5`):** Removi os registros sem preenchimento na nota de Apgar no 5º minuto (`APGAR5`), garantindo a confiabilidade da variável alvo principal.
5. **Imputação Orientada ao Domínio:**
   * Padronizei valores ausentes em `ESTCIVMAE` e `CONSULTAS` atribuindo o código `9` ("Ignorado"), conforme o dicionário oficial do DataSUS.
   * Preenchi os valores faltantes da variável quantitativa `QTDFILVIVO` com `0`.
   * Apliquei critérios de negócio para tratar os valores ausentes das variáveis categóricas restantes com base na distribuição e relevância clínica.
6. **Categorização Clínica e Padronização:**
   * Criei uma nova variável categórica traduzindo a escala numérica de `APGAR5` em faixas de risco de asfixia neonatal (`normal`, `asfixia leve`, `asfixia moderada` e `asfixia severa`) e calculei suas respectivas frequências.
   * Padronizei todos os nomes das colunas para o formato **`snake_case`** (letras minúsculas separadas por *underscore*) com `inplace=True` para facilitar futuras etapas de modelagem.

## 🛠️ Tecnologias Utilizadas
* **Python 3**
* **Pandas** (diagnóstico de missings, imputação, categorização e padronização de schema)
* **Jupyter Notebook / Google Colab**
  

TAREFA 02 -  # 🧹 Módulo 05 - Limpeza, Preparação de Dados e Construção de Target

Repositório dedicado aos projetos práticos do **Módulo 05** do curso **Profissão: Cientista de Dados (EBAC)**, dividido em duas etapas principais: tratamento e higienização de dados públicos de saúde (**DataSUS**) e engenharia da variável resposta (*target*) para modelagem de risco de crédito (**Credit Scoring**).

## 💡 Como elaborei este projeto

Desenvolvi os notebooks aplicando técnicas de preparação de dados voltadas a problemas reais de negócio e saúde pública:

### 1. Limpeza e Preparação de Dados — DataSUS / SINASC (`Tarefa 01`)
* **Auditoria e Diagnóstico de Missings:** Carreguei a base de nascidos vivos (`SINASC_RO_2019.csv`, com 11.509 registros e 69 variáveis), validei a ausência de duplicatas (`.drop_duplicates()`) e mapeei o volume e o percentual de valores ausentes (`.isna().sum()`) em cada coluna.
* **Seleção e Higienização:** Filtrei as 9 variáveis relevantes para o estudo perinatal e removi registros sem preenchimento na variável alvo `APGAR5`.
* **Imputação Orientada ao Domínio:** Atribuí o código `9` ("Ignorado") aos missings de `ESTCIVMAE` e `CONSULTAS` conforme o dicionário do DataSUS, preenchi `QTDFILVIVO` com `0` e tratei os demais campos faltantes com base em critérios clínicos.
* **Categorização e Padronização:** Classifiquei os escores numéricos de `APGAR5` em faixas clínicas de risco de asfixia (`normal`, `asfixia leve`, `asfixia moderada` e `asfixia severa`) e padronizei todos os nomes de colunas para **`snake_case`**.

### 2. Construção da Variável Resposta (*Default*) — Risco de Crédito (`Tarefa 02`)
* **Identificação de Atrasos em Base Larga:** A partir da tabela de histórico mensal (`pagamentos_largo.csv`, de `mes_0` a `mes_24`), utilizei o método `.isin()` para mapear mensalmente os episódios críticos de inadimplência (*default*).
* **Agregação de Desempenho (24 meses):** Consolidei os indicadores mensais por cliente ao longo da janela de 24 meses (`sum(axis=1)`), classificando como **mau pagador (`True`)** quem apresentou ao menos um episódio de default e **bom pagador (`False`)** os demais.
* **Filtro de Expostos e Cruzamento de Bases:** Integrei a base cadastral de solicitações (`application_record.csv`) com a base de pagamentos para filtrar apenas clientes efetivamente expostos ao risco de crédito (excluindo propostas recusadas/desistências) e consolidar a tabela final pronta para modelagem preditiva.

## 🛠️ Tecnologias Utilizadas
* **Python 3**
* **Pandas** (auditoria de missings, imputação, categorização, operações horizontais `axis=1` e junção de tabelas)
* **Jupyter Notebook / Google Colab**
