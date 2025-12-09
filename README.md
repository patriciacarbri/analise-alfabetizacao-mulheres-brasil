# README — Relatório de Análise Exploratória

**Alfabetização de Mulheres no Brasil (Censo 2022 & INEP 2022)**

Resumo rápido

* Notebook que analisa alfabetização por sexo e raça usando: **Censo Demográfico 2022 (IBGE)** + **Censo Escolar 2022 (INEP)**.
* Objetivo: quantificar o *gap* de alfabetização entre mulheres e homens, entender variação por UF/município, e examinar desigualdades raciais e fluxo escolar (matrículas).
* Resultado principal (síntese): **gap nacional ≈ 0.99 pp** (mulheres com vantagem); gaps maiores e mais relevantes no **Nordeste**; desigualdade racial com padrão territorial (Norte e Sul com maior dispersão/impacto populacional).

---

## Estrutura do repositório / notebook

* `notebook.ipynb` — notebook principal com a análise (carregamento, tratamento, estatística e visualizações).
* `data/` — local esperado dos *trusted* parquet:

  * `data/censo_2022_trusted.parquet`
  * `data/inep_2022_trusted.parquet`
* `data/analysis_outputs/` — pasta onde o notebook grava tabelas e figuras (criada automaticamente).
* `requirements.txt` — dependências Python (sugerido).
* `README.md` — este arquivo.

---

## Dependências mínimas


Dependências principais usadas no notebook:

* pandas, numpy
* matplotlib, seaborn, plotly
* statsmodels, scipy
* jupyter


---

## Como executar

1. Coloque os arquivos parquet no caminho indicado (`data/`).
2. Abra e execute células no Jupyter / JupyterLab:

   ```bash
   jupyter lab notebook.ipynb
   ```
3. (Opcional) executar de modo batch com papermill:

   ```bash
   papermill notebook.ipynb output.ipynb -p PATH_BQ data/censo_2022_trusted.parquet -p PATH_INEP data/inep_2022_trusted.parquet
   ```
4. Verifique saída em `data/analysis_outputs/`.

---

## Organização do notebook (capítulos)

1. **Introdução & fontes** — descrição de dados e objetivos.
2. **Carregamento / padronização** — leitura dos parquet e limpeza mínima (tipos, nomes).
3. **Panorama nacional e por UF/município** — taxas, ranking de capitais, mapas/treemap.
4. **Desigualdade racial (mulheres alfabetizadas)** — taxas por raça, Gini ponderado, heatmap.
5. **Microdados INEP: fluxo e matrículas** — análise por etapas (Fundamental I/II, Médio, EJA), transição/permanência por raça.
6. **Inferência estatística** — z-test de proporções por UF, bootstrap para IC do gap (nacional e por UF).
7. **Conclusão e recomendações**.

---

## Principais outputs gerados

* Tabelas consolidadas: `df_uf_stats.csv`, `df_desigualdade.csv`, `df_ranking_municipios.csv`
* Gráficos salvos: treemap, heatmap de taxa por raça, ranking de gaps, mapas (se habilitados).
  (Ver código nas células finais para nomes exatos / formatos.)

---

## Principais achados  

* **Gap nacional**: ~**0.99 pontos percentuais** favoráveis às mulheres (IC bootstrap estreito: ~0.98–1.00 pp).
* **Regional**: Nordeste concentra os maiores gaps por UF (ex.: RN ≈ +5,34 pp; PB ≈ +5,18 pp).
* **Capitais**: altas taxas (>90%) e diferenças de gênero pequenas; diferenças relevantes aparecem principalmente fora dos grandes centros.
* **Desigualdade racial**: não é homogênea — Norte e Sul mostram maior dispersão/impacto populacional; Nordeste tem média menor, mas desigualdade interna relativamente menor.
* **Matrículas (INEP)**: maior desigualdade racial ocorre no **Fundamental I**; quedas de permanência mais fortes para mulheres pretas e indígenas nos saltos para o Ensino Médio.

---

## Observações técnicas / limitações importantes

* **Inconsistência detectada no INEP:** ~**164.746 registros** onde `QT_MAT_BAS` ≠ soma das colunas por raça. Tratei com cautela — recomendo verificação/curadoria se precisar de análises finas por raça no INEP.
* **Tamanho da base:** milhões de observações; alguns passos (bootstrap por UF com muitos *nboot*) são custosos — ajuste `nboot` conforme disponibilidade computacional.
* **P-values**: com amostras muito grandes, p-values serão sempre próximos de zero. Interprete magnitude (gap, IC) em vez de depender só do p-valor.
* **Autodeclaração e cobertura:** Censo = população autodeclarada; INEP = matrículas formais. São fontes distintas (stock vs. fluxo) — o cruzamento exige cuidado interpretativo.

---

## Reprodutibilidade

* O notebook usa `numpy.random.default_rng(seed)` nas simulações. Seed está fixado para reprodutibilidade (ver função `bootstrap_gap_simple`).
* Filtragens e agregações estão explícitas nas células — executar na ordem preserva coerência dos objetos.

---

## Próximos passos (sugestões práticas)

* Verificar e corrigir a inconsistência nas somas por raça no INEP antes de análises raciais detalhadas.
* Analisar gaps por **faixa etária** (ex.: 15–24, 25–34) para ver se padrão de gênero muda por coorte.
* Construir dashboards interativos (Plotly Dash / Streamlit) para explorar município × raça × sexo.
* Aplicar modelagem causal (ex.: regressões com controles socioeconômicos) para entender determinantes do gap.

---

## Contato / autoria

* Autora do ETL e deste notebook: repositório principal:
  `https://github.com/patriciacarbri/etl-censo-bigquery-mulheres-brasil`
* Para alterações no notebook, criar PR no repositório ou abrir issue com: descrição do problema / célula afetada / sugestão de correção.
 