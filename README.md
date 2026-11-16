# Previsão do número semanal de jogadores de Counter-Strike 2 com SARIMA

Análise completa de séries temporais sobre o número de jogadores ativos de **Counter-Strike 2 / CS:GO** na Steam, de 2016 a 2025, com modelagem SARIMA, tratamento de outliers e previsão para 2026, validada com dados reais do primeiro semestre de 2026.

Trabalho final da disciplina de **Séries Temporais** do Departamento de Estatística da **UFSCar** (julho de 2026).

## Resultados principais

- **Modelo final:** SARIMA(1,1,2)(0,0,1)₅₂ ajustado sobre o log da série, com variáveis dummy (`xreg`) para três outliers
- **Validação (treino até 2025, teste nas 25 primeiras semanas de 2026):** MAPE de **5,91%**, MAE ≈ 94,6 mil jogadores, RMSE ≈ 113,5 mil jogadores
- **Outliers identificados e explicados:** mudança de nível em março/2020 (lockdown da COVID-19), mudança transitória em maio/2023 (BLAST.tv Paris Major, último Major do CS:GO) e uma nova mudança de nível com a transição para o CS2
- **Comparação com outros modelos:**

| Modelo       | MAE     | RMSE    | MAPE   |
|--------------|---------|---------|--------|
| SARIMA       | 94 606  | 113 451 | 5,91%  |
| Holt-Winters | 204 310 | 241 509 | 12,97% |
| ETS          | 65 892  | 83 149  | 4,33%  |

O ETS teve o menor erro no teste; o SARIMA superou o Holt-Winters, provavelmente por causa do tratamento explícito dos outliers.

## Etapas da análise

1. **Preparação dos dados:** registros diários do SteamDB agregados em médias semanais (522 semanas) e transformação logarítmica para estabilizar a variância
2. **Análise exploratória:** gráfico da série, estatísticas descritivas, decomposição STL, ACF/PACF
3. **Estacionariedade:** testes ADF e KPSS; série estacionária após uma diferenciação
4. **Detecção de outliers:** `tsoutliers` (AO, LS e TC), com interpretação de cada evento
5. **Modelagem:** identificação manual pelos correlogramas, confirmada por `auto.arima`
6. **Diagnóstico dos resíduos:** Ljung-Box (sem autocorrelação), Shapiro-Wilk e QQ-plot, Breusch-Pagan
7. **Previsão:** 52 semanas à frente com intervalos de 80% e 95%, revertida para a escala original
8. **Validação hold-out** e comparação com Holt-Winters e ETS

## Arquivos

| Arquivo | Conteúdo |
|---|---|
| `serie_temporais_CS_log.ipynb` | Notebook (kernel R) com todo o código e as saídas |
| `Grupo_..._Relatório___Análise_de_dado_real.pdf` | Relatório final com a análise completa e a discussão |

## Ferramentas

**R**: `forecast`, `tseries`, `tsoutliers`, `lmtest`, `dplyr`, `lubridate` · Jupyter Notebook

## Como reproduzir

1. Baixe o histórico de jogadores do CS2 em [SteamDB, app 730](https://steamdb.info/app/730/charts/) e salve como CSV separado por `;` (colunas `DateTime` e `Players`)
2. Instale os pacotes:
   ```r
   install.packages(c("forecast", "tseries", "tsoutliers", "lmtest", "dplyr", "lubridate"))
   ```
3. Abra o notebook com um kernel R (IRkernel) e ajuste o nome do arquivo CSV na célula de leitura

## Autores

- Marcelo Xinhong Huang
- Letícia Yumi Ichibara
- Pedro Henrique de Araujo

Orientação: Profa. Maria Silvia de Assis-Moura (UFSCar)

## Fonte dos dados

[SteamDB: Counter-Strike 2 Charts](https://steamdb.info/app/730/charts/)
