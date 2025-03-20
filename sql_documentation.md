# Documentação de Pré-processamento com SQLite

## Contexto
Este documento registra o uso do SQLite para preprocessar o dataset "Seattle Weather". O objetivo foi agregar os dados climáticos por mês e tipo de clima, calculando médias e contagens para visualizações no Power BI.

---

## Passos e Comandos SQL

### 1. Abertura do Banco de Dados
- **Comando**: `sqlite3 climate.db`  
- **Descrição**: Inicia o SQLite e cria/abre o banco climate.db no diretório onde o CSV está localizado.

### 2. Configuração do Modo CSV
- **Comando**: `.mode csv`  
- **Descrição**: Define o modo CSV para importação e exportação, garantindo compatibilidade com o formato do arquivo.

### 3. Importação do Dataset
- **Comando**: `.import seattle-weather.csv climate`  
- **Descrição**: Importa o arquivo `seattle-weather.csv` como a tabela "climate", usando os cabeçalhos do CSV como nomes de colunas.

### 4. Verificação das Colunas
- **Comando**: `.schema climate`  
- **Descrição**: Exibe a estrutura da tabela "climate", confirmando as colunas ("date", "precipitation", "temp_max", "temp_min", "wind", "weather").

### 5. Query SQL para Seleção de Colunas
- **Comando**:  
  ```sql
  SELECT strftime('%Y-%m', date) AS "Month",
       AVG(precipitation) AS "Avg_Precipitation",
       AVG(temp_max) AS "Avg_Temp_Max",
       AVG(temp_min) AS "Avg_Temp_Min",
       AVG(wind) AS "Avg_Wind",
       weather,
       COUNT(*) AS "Weather_Count"
  FROM climate
  GROUP BY strftime('%Y-%m', date), weather;
- **Descrição**: Calcula médias mensais de precipitação, temperaturas e vento, e conta ocorrências por tipo de clima, agrupando por mês e "weather".

### 6. Configuração para Exportação
- **Comando**:
  ```sql
  .headers on
  .mode csv
- **Descrição**: Ativa cabeçalhos e define o modo CSV para o arquivo de saída.

### 7. Exportação do Resultado
- **Comandos**:
  ```sql
  .output climate_summary.csv
  
  SELECT strftime('%Y-%m', date) AS "Month",
       AVG(precipitation) AS "Avg_Precipitation",
       AVG(temp_max) AS "Avg_Temp_Max",
       AVG(temp_min) AS "Avg_Temp_Min",
       AVG(wind) AS "Avg_Wind",
       weather,
       COUNT(*) AS "Weather_Count"
  FROM climate
  GROUP BY strftime('%Y-%m', date), weather;

  .output stdout
- **Descrição**: Exporta os dados agregados para `climate_summary.csv` e retorna a saída ao Terminal.

### 8. Encerramento do Banco
- **Comando**: 
  ```sql
  .exit
- **Descrição**: Fecha o SQLite, concluindo o preprocessamento.

---

## Resultado
O arquivo `climate_summary.csv` foi gerado com médias mensais e contagens por tipo de clima, pronto para visualizações no Power BI. Este processo facilitou a utilização de valores práticos e processados nos dashboards com agregações (`GROUP BY`, `AVG`, `COUNT`) e manipulação de datas (`strftime`).