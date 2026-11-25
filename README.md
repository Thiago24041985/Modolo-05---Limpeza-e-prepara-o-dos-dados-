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
