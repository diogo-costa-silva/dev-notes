
### 📌 Conceitos-chave

#### 1️⃣ Entidade

#### O que é:

É um objeto ou conceito do mundo real que queremos representar na base de dados.
Uma entidade é uma “coisa” com significado próprio, sobre a qual guardamos informação.

Exemplos no teu projeto MotoGP:

- Piloto
- Construtor
- Circuito
- Corrida
- Categoria

#### Na prática:

> No diagrama ER (modelo entidade-relacionamento), uma entidade transforma-se normalmente numa tabela.

#### Analogia:

> Pensa numa entidade como um “substantivo” — algo que existe por si (Piloto, Circuito, Moto).

<br>

---

<br>

#### 2️⃣ Relacionamento

#### O que é:

Descreve como duas ou mais entidades estão ligadas entre si.

Tipos mais comuns:

- 1:1 (um para um) → Um piloto tem exatamente uma data de nascimento.
- 1:N (um para muitos) → Um construtor pode ter vários pilotos.
- N:N (muitos para muitos) → Um piloto participa em muitas corridas, e cada corrida tem muitos pilotos.

Na prática:

> Em bases de dados relacionais, relações N:N são normalmente resolvidas com uma tabela intermédia (**junction table**).

Analogia:

> Pensa nos relacionamentos como os “verbos” que ligam os substantivos (Piloto participa em Corrida, Circuito está localizado em País).

<br>

---

<br>

#### 3️⃣ Dimensão (no contexto de Data Warehousing e modelos analíticos)

#### O que é:

Uma tabela que contém **atributos descritivos** de uma entidade — usados para **filtrar, agrupar ou segmentar** análises.

#### Características:

- Não armazena medidas numéricas “de negócio” que queremossomar/média.
- É mais estática (não muda tanto no tempo como as tabelasde factos).

Exemplos no MotoGP:

- DimRider (informação sobre pilotos: nome, país, datanascimento…)
- DimCircuit (informação sobre circuitos: nome, país,comprimento…)
- DimClass (categorias como MotoGP, Moto2…)
- DimSeason (ano, número de corridas…)

Analogia:

> Uma dimensão é como o cartão de identidade de algo — **descreve, mas não mede**.

<br>

---

<br>

#### 4️⃣ Tabela de Facto

#### O que é:

Contém eventos/mensurações de negócio e liga-se às dimensões através de chaves estrangeiras.
É onde ficam as métricas que vamos analisar.

Características:

- Contém valores numéricos (“measures”) que podem ser agregados: soma, média, contagem…
- É normalmente muito maior que as tabelas de dimensão.

Exemplos no MotoGP:

- FactRaceResults: posições de cada piloto em cada corrida, voltas rápidas, pontos.
- FactConstructorChampionships: títulos por construtor e ano.
- FactSameNationPodiums: eventos de pódio com pilotos da mesma nação.
  
Analogia:

> Uma tabela de factos é como um livro de registos ou extratos bancários — cada linha é um evento que aconteceu e que podemos medir.

<br>

---

<br>

#### 💡 Resumo visual:

```text
DimRider --------\
DimConstructor --- FactRaceResults --- DimCircuit
DimClass --------/                      \
DimSeason ------------------------------ DimCountry
```

<br>

- As dimensões estão nos lados e descrevem quem, onde, quando, o quê.
- A tabela de factos está no centro e armazena os eventos medíveis.
- Os relacionamentos ligam tudo com chaves primárias/estrangeiras.

<br>

---
---
---


Boa, bora lá 👇

### 1. **Entidade**

* É um **objeto ou conceito do mundo real** sobre o qual queremos guardar informação.
* Exemplos: `Cliente`, `Produto`, `Encomenda`.
* Em bases de dados relacionais, cada entidade normalmente transforma-se numa **tabela**.
* Cada **instância da entidade** corresponde a uma **linha/registo** na tabela.

---

### 2. **Relacionamento**

* Define como as **entidades estão ligadas** entre si.
* Exemplo: um **Cliente** faz uma **Encomenda** → relacionamento 1\:N (um cliente pode ter várias encomendas, mas cada encomenda pertence a um só cliente).
* Em SQL, isto é implementado através de **chaves estrangeiras (foreign keys)**.

---

### 3. **Tabelas de Factos**

* Usadas sobretudo em **Data Warehousing / BI** (modelos estrela ou floco de neve).
* Guardam os **eventos/medidas numéricas** que queremos analisar.
* Exemplos: `Vendas`, `Transações`, `Reservas`.
* Contêm:

  * **Métricas** (ex.: quantidade, preço, valor total)
  * **Chaves estrangeiras** que ligam às tabelas de dimensões

---

### 4. **Tabelas de Dimensões** (o “outro tipo” que estás a referir 😉)

* Fornecem **contexto às tabelas de factos**.
* Exemplos: `Clientes`, `Produtos`, `Tempo`, `Localização`.
* Normalmente contêm **atributos descritivos** (nome, categoria, país, etc.).
* Permitem filtrar e agrupar os factos em análises (ex.: “vendas por mês”, “vendas por país”).

---

⚡ **Resumo rápido em termos de modelação para BI:**

* **Entidade:** objeto que existe no mundo real.
* **Relacionamento:** como os objetos estão ligados.
* **Fact table:** guarda os acontecimentos quantitativos (medidas).
* **Dimension table:** guarda o contexto descritivo para interpretar os factos.

Queres que eu desenhe um mini-exemplo de modelo estrela (com tabelas de factos e dimensões) em SQL para visualizares melhor?
