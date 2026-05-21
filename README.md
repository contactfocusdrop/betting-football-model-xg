```markdown
# 🎯 Betting Football Model - xG Brasil Série A 2026

Modelo de Machine Learning para previsão de resultados e cálculo de Expected Goals (xG) no futebol brasileiro, utilizando dados de eventos em tempo real.

## 📊 Funcionalidades

- **Cálculo de xG** via Regressão Logística (distância do chute)
- **Métricas avançadas** (passe progressivo, carry progressivo, pressão alta)
- **Modelagem Poisson** para probabilidade de Over 2.5 gols
- **Projeções automáticas** para 7 mercados de apostas
- **Visualização profissional** com tabelas estilo scout

## 🚀 Como Usar

### 1. Clone o repositório

```bash
git clone https://github.com/contactfocusdrop/betting-football-model-xg.git
cd betting-football-model-xg
```

### 2. Instale as dependências no R

```r
# Instalar pacotes necessários
packages <- c(
  "tidyverse", "data.table", "lubridate", "janitor", 
  "caret", "xgboost", "glmnet", "yardstick", "gt", "scales"
)

install.packages(packages)
```

### 3. Prepare seus dados

Seu arquivo CSV deve ter estas colunas:

```csv
type_name,team_name,location_x,location_y,minute,shot_outcome_name,pass_end_location_x,pass_end_location_y,carry_end_location_x,carry_end_location_y
Shot,Flamengo,85,45,23,Goal,NA,NA,NA,NA
Pass,Palmeiras,70,50,15,NA,90,48,NA,NA
Pressure,Corinthians,75,40,30,NA,NA,NA,NA,NA
```

### 4. Execute o modelo

```r
# Carregar seus dados
Brasil_A_26 <- read_csv("seu_arquivo.csv")

# Executar todo o código do modelo
source("betting_model.R")
```

## 📈 Exemplo de Output

A tabela gerada mostra:

| TEAM | Projected Goals | Clean Sheet % | Attack Index | Best Market |
|------|----------------|---------------|--------------|-------------|
| Flamengo | 2.15 | 45% | 1.87 | Over 2.5 |
| Palmeiras | 1.92 | 52% | 1.65 | Win to Nil |
| Corinthians | 1.34 | 61% | 0.98 | Under 2.5 |

## 🧠 Como o Modelo Funciona

### 1. Limpeza e Feature Engineering
- Filtra eventos nulos
- Padroniza coordenadas do campo
- Cria variáveis binárias para tipos de evento

### 2. Cálculo do xG
```r
xg_model <- glm(goal ~ shot_distance, family = binomial())
xg <- predict(xg_model, type = "response")
```

### 3. Métricas dos Times
- Total de passes, finalizações, pressões
- Entradas no último terço
- Pressões altas (campo ofensivo)

### 4. Modelo Poisson para Over 2.5
```r
lambda_home <- 1.7  # Média de gols em casa
lambda_away <- 1.2  # Média de gols fora
prob_over25 <- sum(dpois(0:10, lambda_home) * dpois(3:10, lambda_away))
```

## 📁 Estrutura do Projeto

```
betting-football-model-xg/
├── README.md                 # Este arquivo
├── betting_model.R          # Código principal do modelo
├── dados_exemplo.csv        # Exemplo de estrutura de dados
├── team_projection.png      # Screenshot da tabela gerada
├── requirements.txt         # Lista de pacotes R
└── LICENSE                  # MIT License
```

## 🔧 Personalização

### Ajustar parâmetros do modelo

```r
# Alterar limiar de passe progressivo (padrão: 15m)
progressive_pass = ifelse(
  pass_end_location_x - location_x >= 20,  # Aumentado para 20m
  1, 0
)

# Ajustar constante do xG (padrão: 0.11)
projected_goals = round(shots * 0.15, 2)  # Mais conservador

# Modificar limite de Over 2.5
prob_over25 <- matriz %>%
  filter(home + away >= 4) %>%  # Over 3.5 gols
  summarise(prob = sum(prob))
```

## 📊 Mercados Sugeridos

| Mercado | Condição |
|---------|----------|
| Over 2.5 | Gols projetados ≥ 2.0 e Clean Sheet ≤ 40% |
| Win to Nil | Gols ≥ 1.8 e Clean Sheet ≥ 55% |
| Under 2.5 | Gols ≤ 1.2 e Clean Sheet ≥ 60% |
| Corners | Pressões no top 25% |
| BTTS | Demais casos |

## ⚠️ Aviso Legal

**Este modelo é para fins educacionais e de estudo apenas.**

- Apostas esportivas envolvem riscos financeiros significativos
- Nunca aposte mais do que pode perder
- O autor não se responsabiliza por perdas financeiras
- Sempre verifique as odds reais antes de apostar

## 🤝 Como Contribuir

1. Faça um Fork do projeto
2. Crie sua branch (`git checkout -b feature/nova-metrica`)
3. Commit suas mudanças (`git commit -m 'Adiciona métrica X'`)
4. Push para a branch (`git push origin feature/nova-metrica`)
5. Abra um Pull Request

### Ideias para contribuições
- Adicionar modelo de xG com ângulo do chute
- Incluir variáveis de contexto (tempo, altitude, desfalques)
- Criar dashboard interativo com Shiny
- Implementar backtesting histórico

## 📝 Dependências Completas

```r
# requirements.txt para R
tidyverse (>= 2.0.0)
data.table (>= 1.14.0)
lubridate (>= 1.9.0)
janitor (>= 2.2.0)
caret (>= 6.0-94)
xgboost (>= 1.7.0)
glmnet (>= 4.1-8)
yardstick (>= 1.2.0)
gt (>= 0.9.0)
scales (>= 1.2.0)
```

## 📧 Contato

- **Autor**: contactfocusdrop
- **GitHub**: https://github.com/contactfocusdrop
- **Projeto**: https://github.com/contactfocusdrop/betting-football-model-xg

## 📄 Licença

MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes

---

**⭐ Star este projeto se ele foi útil para você!**
```

