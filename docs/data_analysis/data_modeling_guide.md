# 🏗️ Data Modeling Guide

Guia prático para conceitos fundamentais de modelação de dados, com foco em aplicações reais de Business Intelligence e Data Warehousing.

---

## 🎯 **Conceitos Fundamentais**

### 1️⃣ **Entidade**

**O que é:** Um objeto ou conceito do mundo real sobre o qual queremos guardar informação.

**Características:**
- Tem significado próprio no contexto de negócio
- Pode ser concreta (Cliente, Produto) ou abstrata (Encomenda, Evento)
- Transforma-se normalmente numa tabela em bases relacionais

**Exemplos práticos:**
```sql
-- Entidade Cliente
CREATE TABLE Cliente (
    cliente_id INT PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100),
    pais VARCHAR(50),
    data_registo DATE
);
```

> 💡 **Analogia:** Pensa numa entidade como um "substantivo" — algo que existe por si.

### 2️⃣ **Relacionamento**

**O que é:** Define como duas ou mais entidades estão ligadas entre si.

**Tipos de relacionamento:**

| Tipo | Descrição | Exemplo |
|------|-----------|---------|
| **1:1** | Um para um | Um piloto tem uma data de nascimento |
| **1:N** | Um para muitos | Um construtor tem vários pilotos |
| **N:N** | Muitos para muitos | Pilotos participam em várias corridas |

**Implementação em SQL:**
```sql
-- Relacionamento 1:N (Cliente → Encomendas)
CREATE TABLE Encomenda (
    encomenda_id INT PRIMARY KEY,
    cliente_id INT,
    data_encomenda DATE,
    valor_total DECIMAL(10,2),
    FOREIGN KEY (cliente_id) REFERENCES Cliente(cliente_id)
);

-- Relacionamento N:N precisa de tabela intermédia
CREATE TABLE Piloto_Corrida (
    piloto_id INT,
    corrida_id INT,
    posicao_final INT,
    tempo_corrida TIME,
    PRIMARY KEY (piloto_id, corrida_id),
    FOREIGN KEY (piloto_id) REFERENCES Piloto(piloto_id),
    FOREIGN KEY (corrida_id) REFERENCES Corrida(corrida_id)
);
```

> 💡 **Analogia:** Relacionamentos são os "verbos" que ligam os substantivos.

---

## 📊 **Data Warehousing: Fact & Dimension Tables**

### 3️⃣ **Dimension Tables (Dimensões)**

**O que é:** Tabelas que contêm **atributos descritivos** usados para filtrar, agrupar ou segmentar análises.

**Características:**
- ✅ Contêm contexto descritivo
- ✅ Relativamente estáticas (pouco mudança)
- ✅ Usadas para filtros e agrupamentos
- ❌ Não contêm métricas de negócio para agregação

**Exemplos práticos:**
```sql
-- DimPiloto - Informação descritiva sobre pilotos
CREATE TABLE DimPiloto (
    piloto_id INT PRIMARY KEY,
    nome VARCHAR(100),
    pais VARCHAR(50),
    data_nascimento DATE,
    categoria_atual VARCHAR(20),
    equipa_atual VARCHAR(50)
);

-- DimCircuito - Informação sobre circuitos
CREATE TABLE DimCircuito (
    circuito_id INT PRIMARY KEY,
    nome VARCHAR(100),
    pais VARCHAR(50),
    cidade VARCHAR(50),
    comprimento_km DECIMAL(5,2),
    tipo_circuito VARCHAR(20) -- 'Permanente', 'Urbano', etc.
);
```

### 4️⃣ **Fact Tables (Tabelas de Facto)**

**O que é:** Contêm eventos/medições de negócio e ligam-se às dimensões através de chaves estrangeiras.

**Características:**
- ✅ Contêm métricas numéricas agregáveis
- ✅ Grande volume de dados (eventos)
- ✅ Ligadas a múltiplas dimensões
- ✅ Representam factos mensuráveis do negócio

**Exemplos práticos:**
```sql
-- FactResultadoCorrida - Eventos de corrida mensuráveis
CREATE TABLE FactResultadoCorrida (
    resultado_id INT PRIMARY KEY,
    
    -- Chaves estrangeiras (ligações às dimensões)
    piloto_id INT,
    corrida_id INT,
    circuito_id INT,
    temporada_id INT,
    
    -- Métricas (facts)
    posicao_final INT,
    pontos_conquistados INT,
    tempo_total_ms BIGINT,
    voltas_completadas INT,
    melhor_volta_ms BIGINT,
    
    -- Ligações
    FOREIGN KEY (piloto_id) REFERENCES DimPiloto(piloto_id),
    FOREIGN KEY (corrida_id) REFERENCES DimCorrida(corrida_id),
    FOREIGN KEY (circuito_id) REFERENCES DimCircuito(circuito_id)
);
```

---

## ⭐ **Star Schema Model**

### Estrutura Visual:
```
DimPiloto --------\
DimEquipa --------- FactResultadoCorrida ------ DimCircuito
DimTemporada -----/                        \---- DimCorrida
```

### Exemplo Completo:
```sql
-- Query analítica típica: Pontos por piloto português em 2023
SELECT 
    p.nome AS piloto,
    p.pais,
    t.ano,
    SUM(f.pontos_conquistados) AS total_pontos,
    AVG(f.posicao_final) AS posicao_media,
    COUNT(*) AS corridas_participadas
FROM FactResultadoCorrida f
JOIN DimPiloto p ON f.piloto_id = p.piloto_id
JOIN DimTemporada t ON f.temporada_id = t.temporada_id
WHERE p.pais = 'Portugal'
  AND t.ano = 2023
GROUP BY p.nome, p.pais, t.ano
ORDER BY total_pontos DESC;
```

---

## 🛠️ **Melhores Práticas**

### ✅ **Design Guidelines**

1. **Normalização vs Denormalização:**
   - **OLTP (transacional):** Normalizar para evitar redundância
   - **OLAP (analítico):** Denormalizar para performance

2. **Naming Conventions:**
   ```sql
   -- Consistente e claro
   DimCustomer, DimProduct, DimTime
   FactSales, FactInventory
   
   -- Chaves estrangeiras
   customer_id, product_id (não customer_key)
   ```

3. **Data Types Optimization:**
   ```sql
   -- Otimizar para performance e espaço
   categoria VARCHAR(20) NOT NULL,    -- Não VARCHAR(255)
   preco DECIMAL(10,2),              -- Precisão adequada
   data_evento DATE,                 -- Não DATETIME se só precisas da data
   ```

### ⚡ **Performance Tips**

```sql
-- Indexing strategy
CREATE INDEX idx_fact_piloto_temporada ON FactResultadoCorrida(piloto_id, temporada_id);
CREATE INDEX idx_dim_piloto_pais ON DimPiloto(pais);

-- Partitioning por tempo (grandes volumes)
CREATE TABLE FactVendas_2023 PARTITION OF FactVendas
FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');
```

---

## 🔍 **Validation & Testing**

### Data Quality Checks:
```sql
-- Verificar integridade referencial
SELECT 'Órfãos na Fact Table' as check_type, COUNT(*) as count
FROM FactResultadoCorrida f
LEFT JOIN DimPiloto p ON f.piloto_id = p.piloto_id
WHERE p.piloto_id IS NULL;

-- Verificar duplicados na dimensão
SELECT piloto_id, COUNT(*) as duplicates
FROM DimPiloto
GROUP BY piloto_id
HAVING COUNT(*) > 1;

-- Verificar valores em falta
SELECT 
    COUNT(*) as total_records,
    SUM(CASE WHEN pontos_conquistados IS NULL THEN 1 ELSE 0 END) as null_pontos,
    SUM(CASE WHEN posicao_final IS NULL THEN 1 ELSE 0 END) as null_posicao
FROM FactResultadoCorrida;
```

---

## 📈 **Advanced Concepts**

### Slowly Changing Dimensions (SCD)
```sql
-- SCD Tipo 2: Manter histórico de mudanças
CREATE TABLE DimPiloto_SCD2 (
    piloto_key INT PRIMARY KEY,        -- Surrogate key
    piloto_id INT,                     -- Business key
    nome VARCHAR(100),
    equipa VARCHAR(50),
    data_inicio DATE,
    data_fim DATE,
    is_current BOOLEAN DEFAULT TRUE
);
```

### Bridge Tables para N:N
```sql
-- Quando pilotos podem ter múltiplas equipas numa temporada
CREATE TABLE BridgePilotoEquipa (
    piloto_id INT,
    equipa_id INT,
    temporada_id INT,
    data_inicio DATE,
    data_fim DATE,
    PRIMARY KEY (piloto_id, equipa_id, temporada_id)
);
```

---

## 🎯 **Resumo Prático**

| Conceito | Uso Principal | Exemplo |
|----------|---------------|---------|
| **Entidade** | Objetos do negócio | Cliente, Produto, Encomenda |
| **Relacionamento** | Como entidades se ligam | Cliente faz Encomenda (1:N) |
| **Fact Table** | Eventos mensuráveis | Vendas, Transações, Resultados |
| **Dimension Table** | Contexto descritivo | Informação de Cliente, Produto |

### Quick Decision Tree:
```
É algo que queremos medir/contar? 
├─ Sim → Fact Table
└─ Não → Dimension Table

É um objeto de negócio?
├─ Sim → Entidade
└─ Não → Pode ser um atributo

Quantas ligações tem?
├─ 1:N ou N:N → Tabela própria
└─ 1:1 → Pode ser atributo
```

---

> 🚀 **Pro Tip:** Começa sempre com um modelo simples e vai evoluindo. É melhor um modelo funcional que um modelo perfeito que nunca se implementa!