<div align="center">

<img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/GeoPandas-1.1-green?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Leaflet.js-1.9-199900?style=for-the-badge&logo=leaflet&logoColor=white"/>
<img src="https://img.shields.io/badge/GitHub%20Pages-deployed-brightgreen?style=for-the-badge&logo=github&logoColor=white"/>
<img src="https://img.shields.io/badge/Licença-MIT-yellow?style=for-the-badge"/>

# 🌲 Florestas Públicas do Brasil
### Cadastro Nacional de Florestas Públicas (CNFP 2024) — Análise Geoespacial e WebMap Interativo

**[Acessar o WebMap](https://pedroluizskt.github.io/florestas-publicas-brasil/)** · [Dados](./outputs/) · [Notebooks](./notebooks/)

> *39,91% do território brasileiro é floresta pública — mas o que isso significa na prática?*
> *Este repositório transforma 20.829 feições do CNFP em inteligência territorial interativa.*

</div>

---

## Sobre o Projeto

Este projeto realiza a análise geoespacial completa do **Cadastro Nacional de Florestas Públicas (CNFP 2024)**, publicado pelo **Serviço Florestal Brasileiro (SFB/MMA)**, cruzando os dados com os limites municipais e estaduais do IBGE 2024.

O produto final é um **WebMap interativo** publicado via GitHub Pages, que permite:

- Visualizar a distribuição das florestas públicas em escala estadual e municipal
- Analisar a composição por categoria (Assentamentos, UCs, TIs, Florestas Não Destinadas, etc.)
- Explorar a série histórica de criação de florestas públicas desde 1930
- Comparar cobertura por bioma

### Principais Resultados

| Indicador | Valor |
|---|---|
| Área total cadastrada | 339,8 milhões de ha |
| % do território brasileiro | 39,91% |
| Feições cadastradas | 20.829 |
| Municípios abrangidos | 3.438 |
| Maior floresta pública | Alto Rio Negro (AM) — 7,29 M ha |
| Série histórica | 1930–2024 |

---

## 🗺 WebMap

Acesse o WebMap interativo em:
**[https://pedroluizskt.github.io/florestas-publicas-brasil/](https://pedroluizskt.github.io/florestas-publicas-brasil/)**

### Funcionalidades

| Aba | Descrição |
|---|---|
| 🗺 Visão Geral | Choropleth estadual/municipal + camada de florestas com filtro por categoria |
| 🏆 Ranking | Top 50 municípios por área (ha) e por % do território municipal coberto |
| 📈 Histórico | Série acumulada e criações anuais de florestas públicas (1930–2024) |
| 🌿 Biomas | Distribuição por bioma × categoria |

---

## 🗂 Estrutura do Repositório

```
florestas-publicas-brasil/
│
├── notebooks/
│   ├── 01_diagnostico.ipynb       # Inspeção completa dos dados brutos
│   └── 02_processamento.ipynb     # Processamento, estatísticas e geração do WebMap
│
├── outputs/                    # CSVs de cache (reproduzíveis)
│   ├── stats_uf.csv               # Estatísticas por estado
│   ├── stats_uf_legenda.csv       # Área por UF × categoria
│   ├── stats_bioma.csv            # Área por bioma × categoria
│   ├── stats_municipios.csv       # Ranking municipal
│   ├── stats_serie_historica.csv  # Série histórica anual
│   └── kpis_nacionais.csv         # KPIs agregados nacionais
│
├── docs/                       # GitHub Pages (WebMap)
│   ├── index.html                 # WebMap interativo (Leaflet.js + Chart.js)
│   ├── geojson_uf.json            # GeoJSON estados simplificado
│   ├── geojson_municipios.json    # GeoJSON municípios simplificado
│   └── geojson_cnfp.json          # GeoJSON CNFP 2024 simplificado (~75MB)
│
├── data/
│   └── README.md                  # Instruções para download dos dados brutos
│
├── .gitignore
├── requirements.txt
└── README.md
```

> ⚠️ **Dados brutos não versionados:** Os shapefiles originais (CNFP: 732MB, Municípios: 273MB)
> são muito grandes para o Git e devem ser baixados conforme instruções em [`data/README.md`](./data/README.md).

---

## ⚙️ Como Reproduzir

### 1. Pré-requisitos

```bash
git clone https://github.com/seuusuario/florestas-publicas-brasil.git
cd florestas-publicas-brasil
pip install -r requirements.txt
```

### 2. Download dos dados

Siga as instruções em [`data/README.md`](./data/README.md) para baixar os shapefiles originais.

### 3. Executar os notebooks

```bash
jupyter notebook notebooks/
```

Execute **em ordem**:
1. `01_diagnostico.ipynb` — opcional, inspeção exploratória
2. `02_processamento.ipynb` — gera todos os outputs e o WebMap

O notebook `02_processamento.ipynb` gera automaticamente:
- Os CSVs em `outputs/`
- Os GeoJSONs e o `index.html` em `docs/`

### 4. Visualizar localmente

```bash
cd docs
python -m http.server 8080
# Abrir: http://localhost:8080
```

> ⚠️ O WebMap faz `fetch()` dos GeoJSONs — não funciona abrindo o `index.html` diretamente
> como arquivo local. Use um servidor HTTP local (comando acima) ou GitHub Pages.

---

## Metodologia

### Processamento Geoespacial

- **CRS de trabalho:** EPSG:5641 (Cônica Equivalente de Albers / IBGE) para cálculo de áreas
- **CRS de publicação:** EPSG:4674 (SIRGAS 2000 geográfico) para os GeoJSONs
- **Correção de geometrias:** `shapely.validation.make_valid()` aplicado em 826 feições inválidas
- **Simplificação:** tolerância diferenciada por escala (0,02° UFs · 0,01° municípios · 0,005° CNFP)
- **Join municipal:** via campo `geocodigo` (código IBGE 7 dígitos) — sem spatial join

### Validação de Área

A área calculada (339,83 M ha, EPSG:5641) apresenta discrepância de ~1,7%
em relação ao campo `area_ha` do shapefile original (334,14 M ha).
Essa diferença é esperada e decorre da projeção utilizada na origem dos dados.
Ambos os valores são metodologicamente válidos; este projeto utiliza a área
calculada em projeção Albers por maior precisão para análises nacionais.

### ⚠️ Três Ressalvas Técnicas Importantes

**Ressalva 1 — Sobreposições espaciais (a mais crítica)**
O próprio SFB alerta: "no processo de produção do CNFP, existem sobreposições entre alguns polígonos, por exemplo, entre Unidades de Conservação da Natureza (UCs) ou entre UCs e Terras Indígenas." 

**Ressalva 2 — "Floresta pública" ≠ "floresta em pé"**
O SFB explicita: "São cadastradas sumariamente no Cadastro-Geral da União, independentemente de sua cobertura vegetal, do uso da terra e da observação dos estágios de cadastramento, as Terras Indígenas e as Unidades de Conservação federais." 

**Ressalva 3 — Assentamentos como "floresta pública"**
Os Assentamentos representam 49,2% das feições (10.240 registros). O CNFP classifica como Tipo A — ao lado de UCs e TIs — os "Assentamentos Rurais Públicos", destinados ao uso de comunidades tradicionais. Muitos desses assentamentos têm cobertura florestal degradada. A inclusão deles é legalmente correta dentro do escopo do CNFP, mas é uma limitação semântica relevante para qualquer comunicação que use o número de 339,83M ha como proxy de "floresta conservada". 

---

## Fontes de Dados

| Fonte | Dataset | Acesso |
|---|---|---|
| SFB/MMA | CNFP 2024 — Cadastro Nacional de Florestas Públicas | [florestal.gov.br](https://www.florestal.gov.br/cadastro-nacional-de-florestas-publicas) |
| IBGE | Malha Municipal 2024 | [ibge.gov.br](https://www.ibge.gov.br/geociencias/downloads-geociencias.html) |
| IBGE | Malha Estadual 2024 | [ibge.gov.br](https://www.ibge.gov.br/geociencias/downloads-geociencias.html) |

---

## 🛠 Stack Tecnológica

**Processamento:**
`Python 3.10` · `GeoPandas 1.1` · `Shapely` · `Pandas` · `NumPy` · `Matplotlib`

**WebMap:**
`Leaflet.js 1.9` · `Chart.js 4.4` · `Source Sans 3` · `Source Code Pro`

**Publicação:**
`GitHub Pages` · `GeoJSON` · `HTML/CSS/JS` puro (zero dependências de servidor)

---

## Licença

Este projeto está licenciado sob a **MIT License** — veja o arquivo [LICENSE](./LICENSE) para detalhes.

Os dados utilizados são públicos e de domínio governamental (SFB/MMA e IBGE).

---

## 👤 Autor

**Pedro Luiz**
Engenheiro Florestal · Ciência de Dados · Sensoriamento Remoto · WebGIS

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat-square&logo=linkedin)](www.linkedin.com/in/pedro-luiz-rodrigues-vaz-de-melo)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/PedroLuizskt)

---

<div align="center">
<sub>Dados: SFB/MMA · IBGE 2024 · Processado em 27/04/2026</sub>
</div>
