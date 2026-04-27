# 📁 Dados Brutos

Os arquivos de dados originais **não estão versionados** neste repositório
devido ao seu tamanho (>1GB no total). Faça o download manualmente conforme
as instruções abaixo e coloque-os nas pastas indicadas antes de executar os notebooks.

---

## 📥 Download dos Dados

### 1. CNFP 2024 — Cadastro Nacional de Florestas Públicas

**Fonte:** Serviço Florestal Brasileiro (SFB/MMA)
**URL:** https://www.florestal.gov.br/cadastro-nacional-de-florestas-publicas

Baixe o shapefile mais recente e extraia os arquivos para:
```
data/cnfp_2024/
├── cnfp_2024.shp
├── cnfp_2024.dbf
├── cnfp_2024.shx
├── cnfp_2024.prj
└── cnfp_2024.cst
```

### 2. Malha Municipal e Estadual — IBGE 2024

**Fonte:** IBGE — Geociências
**URL:** https://www.ibge.gov.br/geociencias/downloads-geociencias.html
> Organizações do Território → Malhas territoriais → Municípios e Estados → 2024

Extraia os arquivos para:
```
data/BR_Municipios_2024/
├── BR_Municipios_2024.shp
├── BR_Municipios_2024.dbf
├── BR_Municipios_2024.shx
├── BR_Municipios_2024.prj
└── BR_Municipios_2024.cpg

data/BR_UF_2024/
├── BR_UF_2024.shp
├── BR_UF_2024.dbf
├── BR_UF_2024.shx
├── BR_UF_2024.prj
└── BR_UF_2024.cpg
```

---

## 📏 Tamanho dos Arquivos

| Arquivo | Tamanho |
|---|---|
| cnfp_2024.shp | ~732 MB |
| cnfp_2024.dbf | ~92 MB |
| BR_Municipios_2024.shp | ~274 MB |
| BR_UF_2024.shp | ~19 MB |

---

## ⚙️ Configuração dos Caminhos

Após o download, verifique os caminhos no início da **Célula 1** dos notebooks:

```python
BASE_GEO = Path(r'caminho/para/a/pasta/data')
```

Ajuste conforme a localização dos arquivos na sua máquina.

---

## 📋 Versões Utilizadas

| Dataset | Versão | Data de acesso |
|---|---|---|
| CNFP | 2024 | Abr/2026 |
| IBGE Municípios | 2024 | Abr/2026 |
| IBGE UFs | 2024 | Abr/2026 |
