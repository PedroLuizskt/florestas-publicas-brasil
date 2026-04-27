# 📊 Outputs — Estatísticas Derivadas

Arquivos CSV gerados pelo notebook `02_processamento.ipynb`.
São reproduzíveis a partir dos dados brutos e estão versionados
no repositório por serem leves e úteis para análises externas.

## Arquivos

### `stats_uf.csv`
Estatísticas de floresta pública por Unidade Federativa.

| Coluna | Descrição |
|---|---|
| `uf` | Sigla do estado |
| `n_feicoes` | Número de feições cadastradas |
| `area_ha` | Área total de floresta pública (ha) |
| `pct_total` | % do total nacional de FP |
| `area_max_ha` | Maior feição do estado (ha) |
| `area_med_ha` | Área média por feição (ha) |
| `area_uf_ha` | Área total do estado (ha) |
| `pct_uf_coberta` | % do território estadual coberto por FP |

### `stats_uf_legenda.csv`
Matriz UF × Categoria (legenda), com área em ha por célula.

### `stats_bioma.csv`
Matriz Bioma × Categoria (legenda), com área em ha por célula.

### `stats_municipios.csv`
Ranking de municípios por área de floresta pública.

| Coluna | Descrição |
|---|---|
| `cd_mun` | Código IBGE do município (7 dígitos) |
| `municipio` | Nome do município |
| `uf` | Estado |
| `n_feicoes` | Feições cadastradas |
| `area_ha` | Área de FP no município (ha) |
| `area_mun_ha` | Área total do município (ha) |
| `pct_mun_coberta` | % do município coberto por FP |

### `stats_serie_historica.csv`
Série histórica anual de criação de florestas públicas.

| Coluna | Descrição |
|---|---|
| `ano` | Ano de criação |
| `n_criadas` | Feições criadas no ano |
| `area_ha` | Área criada no ano (ha) |
| `area_acum_ha` | Área acumulada até o ano (ha) |

### `kpis_nacionais.csv`
KPIs nacionais agregados para o WebMap.

## Metodologia de Área

Áreas calculadas em **EPSG:5641** (Cônica Equivalente de Albers / IBGE).
Diferença de ~1,7% em relação ao campo `area_ha` original do shapefile
é esperada e decorre da projeção utilizada na geração do dado original.
