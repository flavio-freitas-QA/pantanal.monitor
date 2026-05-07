# 🌿 Pantanal Monitor — Análise Ambiental

Análise de dados ambientais do Pantanal (Janeiro/2025), com tratamento de valores ausentes, estatísticas descritivas, gráficos estáticos em Python e dashboard interativo em JavaScript.

---

## 📁 Estrutura do Projeto

```
pantanal_analysis/
├── dados_pantanal.csv       # Dados brutos originais
├── analise.py               # Script Python de análise
├── README.md                # Este arquivo
└── output/
    ├── dados_tratados.json  # Dados tratados (gerado pelo Python)
    ├── temperatura_ndvi.png # Gráfico 1: Temperatura + NDVI (gerado pelo Python)
    ├── nivel_rio.png        # Gráfico 2: Nível do Rio (gerado pelo Python)
    └── index.html           # Dashboard interativo (JS/Chart.js)
```

---

## ⚙️ Requisitos

- Python 3.8+
- Bibliotecas: `pandas`, `matplotlib`

Instalação das dependências:
```bash
pip install pandas matplotlib
```

---

## ▶️ Como Executar

### 1. Análise Python (gráficos estáticos + JSON)
```bash
# Na raiz do projeto:
python analise.py
```

O script irá:
- Ler `dados_pantanal.csv`
- Tratar valores ausentes via interpolação linear
- Exibir estatísticas no terminal
- Gerar `output/dados_tratados.json`
- Gerar `output/temperatura_ndvi.png` e `output/nivel_rio.png`

### 2. Dashboard Interativo (JS)
Abra diretamente no navegador:
```bash
# macOS
open output/index.html

# Linux
xdg-open output/index.html

# Windows
start output/index.html
```

> Não é necessário servidor local — o HTML carrega o Chart.js via CDN e os dados estão embutidos.

---

## 🧠 Decisões Técnicas

### Tratamento de Valores Ausentes — Interpolação Linear
Os campos `nivel_rio_m` e `ndvi` possuem 2 valores ausentes cada (dias 03, 07 e 04, 09 respectivamente).

**Escolha: `interpolate(method='linear')` do pandas**

- Adequada para séries temporais com tendência contínua (nível de rio e índice de vegetação variam suavemente ao longo do tempo)
- Alternativas como `fillna(mean)` ignorariam a tendência temporal; `ffill/bfill` apenas repetiria o valor anterior/seguinte
- Os valores interpolados são sinalizados com `★` tanto no gráfico quanto na tabela

### Visualizações Python (Matplotlib)
- **Gráfico 1 — Temperatura + NDVI (duplo eixo Y):** permite comparar visualmente as duas variáveis em escalas distintas, revelando a correlação positiva entre calor e vigor vegetal
- **Gráfico 2 — Nível do Rio (área + linha de média):** destaca a tendência de subida e queda ao longo da quinzena, com a média como referência

### Dashboard JavaScript (Chart.js)
- **4 gráficos interativos:** linha de temperatura, linha de NDVI, barras do nível do rio, e scatter de correlação Temperatura × NDVI
- Valores interpolados são destacados em **âmbar** em todos os gráficos e na tabela
- **Sem dependência de servidor:** o HTML é completamente standalone
- **Tema dark** inspirado na paleta verde/escura do Pantanal

### Exportação JSON
O `analise.py` exporta `output/dados_tratados.json` como contrato entre o backend Python e qualquer frontend, tornando o projeto extensível (ex: consumir via `fetch()` em vez de dados embutidos no HTML).

---

## 📊 Estatísticas dos Dados Tratados

| Variável        | Média  | Mín    | Máx    | Desvio |
|-----------------|--------|--------|--------|--------|
| Temperatura (°C)| 33.95  | 31.80  | 36.00  | 1.41   |
| Nível Rio (m)   | 4.49   | 4.20   | 4.80   | 0.20   |
| NDVI            | 0.6815 | 0.6500 | 0.7200 | 0.0231 |

---

## 🗺️ Contexto dos Dados

| Variável | Descrição |
|----------|-----------|
| `temperatura_c` | Temperatura do ar em graus Celsius |
| `nivel_rio_m`   | Nível do rio em metros |
| `ndvi`          | Índice de Vegetação por Diferença Normalizada (0–1) — quanto mais alto, mais densa a vegetação |

---

*Desenvolvido como exercício de análise de dados ambientais do Pantanal Mato-Grossense.*
