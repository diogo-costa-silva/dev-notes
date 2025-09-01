# 💾 SQL Templates & Queries

Templates, snippets e queries SQL para tarefas comuns de análise de dados.

## 🔍 Common Query Patterns

Templates SQL para análises frequentes:

### Data Exploration
```sql
-- Template para exploração inicial de tabelas
SELECT 
    COUNT(*) as total_rows,
    COUNT(DISTINCT column_name) as distinct_values,
    MIN(date_column) as earliest_date,
    MAX(date_column) as latest_date
FROM table_name;
```

### Data Quality Checks
```sql
-- Verificar dados em falta e duplicados
SELECT 
    SUM(CASE WHEN column_name IS NULL THEN 1 ELSE 0 END) as null_count,
    COUNT(*) - COUNT(DISTINCT id) as duplicate_count
FROM table_name;
```

## 📊 Analytics Queries

### Window Functions
- **Ranking** e percentiles
- **Moving averages** e trend analysis
- **Lag/Lead** para comparações temporais
- **Cumulative metrics** e running totals

### Aggregation Patterns
- **GROUP BY** com múltiplas dimensões
- **PIVOT** e UNPIVOT operations
- **Conditional aggregation** com CASE
- **Statistical functions** (STDDEV, VARIANCE, etc.)

## 🚀 Performance Optimization

Templates para queries otimizadas:

- **Index usage** strategies
- **Query hints** para performance
- **Partitioning** queries
- **Batch processing** patterns

## 🔧 Database-Specific

Templates específicos por DB:

- **PostgreSQL** advanced features
- **MySQL** optimization tricks  
- **SQLite** embedded scenarios
- **BigQuery** cloud analytics

---

> ⚡ **Performance Tip:** Sempre testa queries com volumes de dados reais antes de colocar em produção!