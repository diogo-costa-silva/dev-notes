# 💾 SQL Templates & Query Library

Templates SQL prontos a usar para análise de dados, otimização de queries e tarefas comuns de Data Analysis.

---

## 🔍 **Data Exploration Templates**

### Basic Data Profiling
```sql
-- Template para exploração inicial de qualquer tabela
SELECT 
    -- Contadores básicos
    COUNT(*) as total_rows,
    COUNT(DISTINCT id_column) as unique_ids,
    
    -- Data range (se existir coluna de data)
    MIN(date_column) as earliest_date,
    MAX(date_column) as latest_date,
    
    -- Missing values check
    COUNT(*) - COUNT(column_name) as missing_values,
    ROUND(100.0 * (COUNT(*) - COUNT(column_name)) / COUNT(*), 2) as missing_percentage
    
FROM your_table_name;
```

### Data Quality Assessment
```sql
-- Verificar qualidade dos dados
WITH quality_check AS (
    SELECT 
        -- Duplicados
        COUNT(*) as total_records,
        COUNT(*) - COUNT(DISTINCT id_column) as duplicate_count,
        
        -- Missing values por coluna importante
        SUM(CASE WHEN column1 IS NULL THEN 1 ELSE 0 END) as column1_nulls,
        SUM(CASE WHEN column2 IS NULL THEN 1 ELSE 0 END) as column2_nulls,
        
        -- Outliers básicos (para colunas numéricas)
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY numeric_column) as q1,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY numeric_column) as q3
        
    FROM your_table_name
)
SELECT 
    *,
    ROUND(100.0 * duplicate_count / total_records, 2) as duplicate_percentage,
    q3 + 1.5 * (q3 - q1) as outlier_upper_bound,
    q1 - 1.5 * (q3 - q1) as outlier_lower_bound
FROM quality_check;
```

---

## 🔗 **Join Validation Templates**

### Foreign Key Coverage Check
```sql
-- Verificar cobertura de foreign keys antes de fazer joins
SELECT 
    'table1' as source_table,
    COUNT(*) as total_records,
    COUNT(t2.id) as matched_records,
    COUNT(*) - COUNT(t2.id) as orphan_records,
    ROUND(100.0 * COUNT(t2.id) / COUNT(*), 2) as match_rate,
    CASE 
        WHEN COUNT(t2.id) = COUNT(*) THEN 'Perfect Match'
        WHEN COUNT(t2.id) > COUNT(*) * 0.95 THEN 'Good Match'
        WHEN COUNT(t2.id) > COUNT(*) * 0.80 THEN 'Acceptable Match'
        ELSE 'Poor Match - Investigate'
    END as match_quality
FROM table1 t1
LEFT JOIN table2 t2 ON t1.foreign_key = t2.id;
```

### Relationship Cardinality Analysis
```sql
-- Detectar tipo de relacionamento entre tabelas
WITH cardinality_check AS (
    SELECT 
        COUNT(*) as total_joins,
        COUNT(DISTINCT t1.id) as unique_t1_ids,
        COUNT(DISTINCT t2.id) as unique_t2_ids,
        COUNT(DISTINCT t1.join_key) as unique_t1_keys,
        COUNT(DISTINCT t2.join_key) as unique_t2_keys
    FROM table1 t1
    JOIN table2 t2 ON t1.join_key = t2.join_key
)
SELECT 
    *,
    CASE 
        WHEN unique_t1_keys = unique_t1_ids AND unique_t2_keys = unique_t2_ids 
        THEN '1:1 Relationship'
        WHEN unique_t1_keys = unique_t1_ids 
        THEN '1:N Relationship (t1 → t2)'
        WHEN unique_t2_keys = unique_t2_ids 
        THEN 'N:1 Relationship (t1 ← t2)'
        ELSE 'N:N Relationship'
    END as relationship_type
FROM cardinality_check;
```

---

## 📊 **Analytics Query Templates**

### Time Series Analysis
```sql
-- Template para análise temporal com múltiplas granularidades
SELECT 
    -- Diferentes granularidades temporais
    DATE_TRUNC('year', date_column) as year,
    DATE_TRUNC('quarter', date_column) as quarter,
    DATE_TRUNC('month', date_column) as month,
    DATE_TRUNC('week', date_column) as week,
    
    -- Métricas principais
    COUNT(*) as record_count,
    SUM(value_column) as total_value,
    AVG(value_column) as average_value,
    STDDEV(value_column) as std_deviation,
    
    -- Percentis
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY value_column) as median,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY value_column) as p95
    
FROM your_table_name
WHERE date_column >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY 1, 2, 3, 4
ORDER BY year, quarter, month, week;
```

### Cohort Analysis Template
```sql
-- Template para análise de coortes (exemplo: retenção de utilizadores)
WITH user_cohorts AS (
    SELECT 
        user_id,
        DATE_TRUNC('month', first_activity_date) as cohort_month,
        DATE_TRUNC('month', activity_date) as activity_month
    FROM user_activities
),
cohort_sizes AS (
    SELECT 
        cohort_month,
        COUNT(DISTINCT user_id) as cohort_size
    FROM user_cohorts
    GROUP BY cohort_month
),
cohort_periods AS (
    SELECT 
        cohort_month,
        activity_month,
        EXTRACT(EPOCH FROM age(activity_month, cohort_month))/2629746 as period_number,
        COUNT(DISTINCT user_id) as active_users
    FROM user_cohorts
    GROUP BY cohort_month, activity_month
)
SELECT 
    cp.cohort_month,
    cs.cohort_size,
    cp.period_number,
    cp.active_users,
    ROUND(100.0 * cp.active_users / cs.cohort_size, 2) as retention_rate
FROM cohort_periods cp
JOIN cohort_sizes cs ON cp.cohort_month = cs.cohort_month
ORDER BY cp.cohort_month, cp.period_number;
```

---

## 🏆 **Advanced Window Functions**

### Ranking & Running Totals
```sql
-- Template completo com window functions para rankings e totais acumulados
SELECT 
    category,
    sub_category,
    date_column,
    value_column,
    
    -- Rankings
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY value_column DESC) as row_num,
    RANK() OVER (PARTITION BY category ORDER BY value_column DESC) as rank_with_ties,
    DENSE_RANK() OVER (PARTITION BY category ORDER BY value_column DESC) as dense_rank,
    PERCENT_RANK() OVER (PARTITION BY category ORDER BY value_column DESC) as percentile_rank,
    
    -- Moving calculations
    SUM(value_column) OVER (
        PARTITION BY category 
        ORDER BY date_column 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) as running_total,
    
    AVG(value_column) OVER (
        PARTITION BY category 
        ORDER BY date_column 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) as moving_avg_7_periods,
    
    -- Lag and Lead
    LAG(value_column, 1) OVER (PARTITION BY category ORDER BY date_column) as previous_value,
    LEAD(value_column, 1) OVER (PARTITION BY category ORDER BY date_column) as next_value,
    
    -- Growth calculations
    value_column - LAG(value_column, 1) OVER (PARTITION BY category ORDER BY date_column) as absolute_change,
    
    ROUND(
        100.0 * (value_column - LAG(value_column, 1) OVER (PARTITION BY category ORDER BY date_column)) / 
        NULLIF(LAG(value_column, 1) OVER (PARTITION BY category ORDER BY date_column), 0), 
        2
    ) as percentage_change

FROM your_table_name
ORDER BY category, date_column;
```

### Segmentation Template
```sql
-- Template para segmentação automática baseada em percentis
WITH segment_boundaries AS (
    SELECT 
        PERCENTILE_CONT(0.33) WITHIN GROUP (ORDER BY value_column) as low_threshold,
        PERCENTILE_CONT(0.67) WITHIN GROUP (ORDER BY value_column) as high_threshold
    FROM your_table_name
),
segmented_data AS (
    SELECT 
        *,
        CASE 
            WHEN value_column <= sb.low_threshold THEN 'Low Value'
            WHEN value_column <= sb.high_threshold THEN 'Medium Value'
            ELSE 'High Value'
        END as value_segment,
        
        NTILE(10) OVER (ORDER BY value_column) as decile,
        NTILE(4) OVER (ORDER BY value_column) as quartile
        
    FROM your_table_name
    CROSS JOIN segment_boundaries sb
)
SELECT 
    value_segment,
    quartile,
    decile,
    COUNT(*) as record_count,
    MIN(value_column) as min_value,
    MAX(value_column) as max_value,
    AVG(value_column) as avg_value,
    SUM(value_column) as total_value
FROM segmented_data
GROUP BY value_segment, quartile, decile
ORDER BY quartile, decile;
```

---

## 🔧 **Database Administration Templates**

### Performance Analysis
```sql
-- Template para análise de performance de queries (PostgreSQL)
SELECT 
    query,
    calls,
    total_time,
    total_time/calls as avg_time_per_call,
    rows,
    100.0 * shared_blks_hit / nullif(shared_blks_hit + shared_blks_read, 0) as hit_percent
FROM pg_stat_statements
WHERE calls > 100  -- Apenas queries executadas frequentemente
ORDER BY total_time DESC
LIMIT 20;
```

### Table Size Analysis
```sql
-- Template para análise de tamanhos de tabelas
SELECT 
    schemaname,
    tablename,
    attname as column_name,
    n_distinct,
    correlation,
    most_common_vals,
    most_common_freqs,
    histogram_bounds
FROM pg_stats
WHERE schemaname = 'your_schema'
  AND tablename = 'your_table'
ORDER BY n_distinct DESC;

-- Table sizes
SELECT 
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size,
    pg_total_relation_size(schemaname||'.'||tablename) as size_bytes
FROM pg_tables
WHERE schemaname NOT IN ('information_schema', 'pg_catalog')
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

---

## 📈 **Business Intelligence Templates**

### KPI Dashboard Query
```sql
-- Template para dashboard de KPIs com comparação período anterior
WITH current_period AS (
    SELECT 
        COUNT(DISTINCT customer_id) as active_customers,
        SUM(revenue) as total_revenue,
        AVG(revenue) as avg_revenue_per_transaction,
        COUNT(*) as total_transactions
    FROM sales_fact
    WHERE date_column >= DATE_TRUNC('month', CURRENT_DATE)
      AND date_column < DATE_TRUNC('month', CURRENT_DATE + INTERVAL '1 month')
),
previous_period AS (
    SELECT 
        COUNT(DISTINCT customer_id) as active_customers,
        SUM(revenue) as total_revenue,
        AVG(revenue) as avg_revenue_per_transaction,
        COUNT(*) as total_transactions
    FROM sales_fact
    WHERE date_column >= DATE_TRUNC('month', CURRENT_DATE - INTERVAL '1 month')
      AND date_column < DATE_TRUNC('month', CURRENT_DATE)
)
SELECT 
    'Active Customers' as metric,
    cp.active_customers as current_value,
    pp.active_customers as previous_value,
    cp.active_customers - pp.active_customers as absolute_change,
    ROUND(100.0 * (cp.active_customers - pp.active_customers) / pp.active_customers, 2) as percentage_change
FROM current_period cp, previous_period pp

UNION ALL

SELECT 
    'Total Revenue' as metric,
    cp.total_revenue as current_value,
    pp.total_revenue as previous_value,
    cp.total_revenue - pp.total_revenue as absolute_change,
    ROUND(100.0 * (cp.total_revenue - pp.total_revenue) / pp.total_revenue, 2) as percentage_change
FROM current_period cp, previous_period pp

UNION ALL

SELECT 
    'Avg Revenue/Transaction' as metric,
    cp.avg_revenue_per_transaction as current_value,
    pp.avg_revenue_per_transaction as previous_value,
    cp.avg_revenue_per_transaction - pp.avg_revenue_per_transaction as absolute_change,
    ROUND(100.0 * (cp.avg_revenue_per_transaction - pp.avg_revenue_per_transaction) / pp.avg_revenue_per_transaction, 2) as percentage_change
FROM current_period cp, previous_period pp;
```

---

## 🚀 **Query Optimization Templates**

### Index Usage Analysis
```sql
-- Verificar uso de índices (PostgreSQL)
SELECT 
    schemaname,
    tablename,
    indexname,
    idx_scan as index_scans,
    idx_tup_read as tuples_read,
    idx_tup_fetch as tuples_fetched,
    pg_size_pretty(pg_relation_size(indexname::regclass)) as index_size
FROM pg_stat_user_indexes
JOIN pg_indexes USING (schemaname, tablename, indexname)
WHERE idx_scan = 0  -- Índices nunca usados
   OR idx_scan < 10  -- Índices pouco usados
ORDER BY pg_relation_size(indexname::regclass) DESC;
```

### Query Rewrite Examples
```sql
-- ❌ Evitar (subquery correlacionada)
SELECT customer_id, customer_name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id 
    AND o.order_date > '2023-01-01'
);

-- ✅ Melhor (JOIN)
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_date > '2023-01-01';

-- ❌ Evitar (função na condição WHERE)
SELECT * FROM sales WHERE EXTRACT(YEAR FROM sale_date) = 2023;

-- ✅ Melhor (condição sargable)
SELECT * FROM sales 
WHERE sale_date >= '2023-01-01' 
  AND sale_date < '2024-01-01';
```

---

## 🔄 **Data Pipeline Templates**

### Incremental Data Loading
```sql
-- Template para carregamento incremental de dados
WITH max_timestamp AS (
    SELECT COALESCE(MAX(last_updated), '1900-01-01'::timestamp) as last_load_time
    FROM target_table
),
new_records AS (
    SELECT *
    FROM source_table s
    CROSS JOIN max_timestamp m
    WHERE s.last_updated > m.last_load_time
)
-- UPSERT (INSERT + UPDATE)
INSERT INTO target_table 
SELECT * FROM new_records
ON CONFLICT (primary_key_column)
DO UPDATE SET 
    column1 = EXCLUDED.column1,
    column2 = EXCLUDED.column2,
    last_updated = EXCLUDED.last_updated;
```

### Data Validation Template
```sql
-- Template para validação de dados após ETL
WITH validation_results AS (
    SELECT 
        'Row Count Check' as validation_type,
        COUNT(*) as actual_value,
        :expected_row_count as expected_value,
        CASE WHEN COUNT(*) = :expected_row_count THEN 'PASS' ELSE 'FAIL' END as status
    FROM target_table
    
    UNION ALL
    
    SELECT 
        'Null Check - Critical Columns' as validation_type,
        SUM(CASE WHEN critical_column IS NULL THEN 1 ELSE 0 END) as actual_value,
        0 as expected_value,
        CASE WHEN SUM(CASE WHEN critical_column IS NULL THEN 1 ELSE 0 END) = 0 THEN 'PASS' ELSE 'FAIL' END as status
    FROM target_table
    
    UNION ALL
    
    SELECT 
        'Duplicate Check' as validation_type,
        COUNT(*) - COUNT(DISTINCT primary_key) as actual_value,
        0 as expected_value,
        CASE WHEN COUNT(*) = COUNT(DISTINCT primary_key) THEN 'PASS' ELSE 'FAIL' END as status
    FROM target_table
)
SELECT 
    validation_type,
    actual_value,
    expected_value,
    status,
    CURRENT_TIMESTAMP as validation_time
FROM validation_results;
```

---

## 📋 **Quick Reference**

### Common Patterns
```sql
-- Top N por grupo
SELECT * FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY group_col ORDER BY value_col DESC) as rn
    FROM table_name
) ranked WHERE rn <= 5;

-- Pivoting data
SELECT customer_id,
    SUM(CASE WHEN product_category = 'A' THEN sales_amount ELSE 0 END) as category_a_sales,
    SUM(CASE WHEN product_category = 'B' THEN sales_amount ELSE 0 END) as category_b_sales
FROM sales_data
GROUP BY customer_id;

-- Unpivoting data
SELECT customer_id, 'category_a' as category, category_a_sales as sales_amount
FROM pivoted_table WHERE category_a_sales IS NOT NULL
UNION ALL
SELECT customer_id, 'category_b' as category, category_b_sales as sales_amount
FROM pivoted_table WHERE category_b_sales IS NOT NULL;
```

---

> 🎯 **Pro Tips:**
> - Sempre usa `EXPLAIN ANALYZE` para verificar performance
> - Testa queries com volumes reais antes de pôr em produção
> - Usa CTEs para queries complexas (mais legível)
> - Prefere JOINs a subqueries correlacionadas
> - Indexa as colunas usadas em WHERE, JOIN e ORDER BY