Com base no material fornecido, o exercício implícito nos slides da Aula 006 é a aplicação completa do algoritmo **PRISM** ao conjunto de dados de recomendação de lentes de contato para gerar regras de classificação para todas as classes (`hard`, `soft` e `none`).

Abaixo está a resolução do exercício em formato Markdown, detalhando o conjunto final de regras e o resumo dos passos de indução para as classes que ficaram pendentes nos slides.

### Conjunto Final de Regras (Algoritmo PRISM)

O algoritmo busca maximizar a probabilidade de classificação correta para uma classe específica em cada iteração. O resultado da indução para as três classes é o seguinte:

**Classe `hard`**

- **R1**: IF astigmatism = yes AND tear production rate = normal AND spectacle prescription = myope THEN lenses = hard
- **R2**: IF age = young AND astigmatism = yes AND tear production rate = normal THEN lenses = hard

**Classe `soft`**

- **R1**: IF astigmatism = no AND tear production rate = normal AND spectacle prescription = hypermetrope THEN lenses = soft
- **R2**: IF astigmatism = no AND tear production rate = normal AND age = young THEN lenses = soft
- **R3**: IF age = pre-presbyopic AND astigmatism = no AND tear production rate = normal THEN lenses = soft

**Classe `none`**

- **R1**: IF tear production rate = reduced THEN lenses = none
- **R2**: IF age = presbyopic AND tear production rate = normal AND spectacle prescription = myope AND astigmatism = no THEN lenses = none
- **R3**: IF spectacle prescription = hypermetrope AND astigmatism = yes AND age = pre-presbyopic THEN lenses = none
- **R4**: IF age = presbyopic AND spectacle prescription = hypermetrope AND astigmatism = yes THEN lenses = none

---

### Detalhamento da Indução das Regras

A regra para a classe `hard` foi detalhada nos slides. Abaixo estão os passos de refinamento extraídos do documento de solução para as classes `soft` e `none`. Em casos de empate nas probabilidades, adotou-se o critério de escolher a opção com maior cobertura positiva.

#### Classe Alvo: `lenses = soft`

Após concluir a classe `hard`, o conjunto S é reiniciado com os 24 exemplos originais.

- **Regra 1:**
  - Passo 1: Seleciona-se `astigmatism = no` (probabilidade: 5/12 = 0.42).
  - Passo 2: Seleciona-se `tear production rate = normal` (probabilidade: 5/6 = 0.83).
  - Passo 3: Seleciona-se `spectacle prescription = hypermetrope` (probabilidade: 3/3 = 1.00).
  - _Regra obtida:_ IF astigmatism = no AND tear production rate = normal AND spectacle prescription = hypermetrope THEN lenses = soft (cobre 3 exemplos).
- **Regra 2:**
  - Passo 1: Seleciona-se `astigmatism = no` (probabilidade: 2/9 = 0.22).
  - Passo 2: Seleciona-se `tear production rate = normal` (probabilidade: 2/3 = 0.67).
  - Passo 3: Seleciona-se `age = young` (probabilidade: 1/1 = 1.00).
  - _Regra obtida:_ IF astigmatism = no AND tear production rate = normal AND age = young THEN lenses = soft (cobre 1 exemplo).
- **Regra 3:**
  - Passo 1: Seleciona-se `age = pre-presbyopic` (probabilidade: 1/7 = 0.14).
  - Passo 2: Seleciona-se `astigmatism = no` (probabilidade: 1/3 = 0.33).
  - Passo 3: Seleciona-se `tear production rate = normal` (probabilidade: 1/1 = 1.00).
  - _Regra obtida:_ IF age = pre-presbyopic AND astigmatism = no AND tear production rate = normal THEN lenses = soft (cobre 1 exemplo).

#### Classe Alvo: `lenses = none`

O conjunto S é novamente reiniciado para induzir as regras da última classe.

- **Regra 1:**
  - Passo 1: Seleciona-se `tear production rate = reduced` (probabilidade: 12/12 = 1.00).
  - _Regra obtida:_ IF tear production rate = reduced THEN lenses = none (cobre 12 exemplos).
- **Regra 2:**
  - Passo 1: Seleciona-se `age = presbyopic` (probabilidade: 2/4 = 0.50).
  - Passo 2: Seleciona-se `tear production rate = normal` (probabilidade: 2/4 = 0.50).
  - Passo 3: Seleciona-se `spectacle prescription = myope` (probabilidade: 1/2 = 0.50).
  - Passo 4: Seleciona-se `astigmatism = no` (probabilidade: 1/1 = 1.00).
  - _Regra obtida:_ IF age = presbyopic AND tear production rate = normal AND spectacle prescription = myope AND astigmatism = no THEN lenses = none (cobre 1 exemplo).
- **Regra 3:**
  - Passo 1: Seleciona-se `spectacle prescription = hypermetrope` (probabilidade: 2/6 = 0.33).
  - Passo 2: Seleciona-se `astigmatism = yes` (probabilidade: 2/3 = 0.67).
  - Passo 3: Seleciona-se `age = pre-presbyopic` (probabilidade: 1/1 = 1.00).
  - _Regra obtida:_ IF spectacle prescription = hypermetrope AND astigmatism = yes AND age = pre-presbyopic THEN lenses = none (cobre 1 exemplo).
- **Regra 4:**
  - Passo 1: Seleciona-se `age = presbyopic` (probabilidade: 1/3 = 0.33).
  - Passo 2: Seleciona-se `spectacle prescription = hypermetrope` (probabilidade: 1/2 = 0.50).
  - Passo 3: Seleciona-se `astigmatism = yes` (probabilidade: 1/1 = 1.00).
  - _Regra obtida:_ IF age = presbyopic AND spectacle prescription = hypermetrope AND astigmatism = yes THEN lenses = none (cobre 1 exemplo).
