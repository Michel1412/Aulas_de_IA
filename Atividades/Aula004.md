### **Exercício 1: Tabela de Probabilidades do Naive Bayes (Suavização de Laplace $\alpha=1$)**

Para construir o modelo Naive Bayes, precisamos calcular a probabilidade _a priori_ de cada classe e as probabilidades condicionais de cada atributo dada a classe. Como foi exigida a suavização laplaciana com $\alpha = 1$, utilizamos a fórmula:
$$P(x_i|v_j) = \frac{\text{contagem}(x_i, v_j) + 1}{\text{contagem}(v_j) + |V_{atributo}|}$$

Considerando o total de 15 instâncias no conjunto de dados "Lentes de Contato":

**1. Probabilidades a Priori (Classes)**

- $|V_{classes}| = 3$ (nenhuma, gelatinosa, dura)
- Total com suavização do denominador: $N + 3 = 15 + 3 = 18$

| Classe         | Contagem | Probabilidade Suavizada $P(C)$ |
| :------------- | :------: | :----------------------------: |
| **nenhuma**    |    3     |   $(3+1)/18 = \mathbf{4/18}$   |
| **gelatinosa** |    8     |   $(8+1)/18 = \mathbf{9/18}$   |
| **dura**       |    4     |   $(4+1)/18 = \mathbf{5/18}$   |

**2. Probabilidades Condicionais (Atributos)**

- **Idade** ($|V| = 3$: infantil, adolescente, adulto)

| Idade           | nenhuma (Total base=3)       | gelatinosa (Total base=8)     | dura (Total base=4)          |
| :-------------- | :--------------------------- | :---------------------------- | :--------------------------- |
| **infantil**    | $(1+1)/(3+3) = \mathbf{2/6}$ | $(2+1)/(8+3) = \mathbf{3/11}$ | $(1+1)/(4+3) = \mathbf{2/7}$ |
| **adolescente** | $(1+1)/(3+3) = \mathbf{2/6}$ | $(2+1)/(8+3) = \mathbf{3/11}$ | $(2+1)/(4+3) = \mathbf{3/7}$ |
| **adulto**      | $(1+1)/(3+3) = \mathbf{2/6}$ | $(4+1)/(8+3) = \mathbf{5/11}$ | $(1+1)/(4+3) = \mathbf{2/7}$ |

- **Diagnóstico** ($|V| = 2$: miopia, hipermetropia)

| Diagnóstico       | nenhuma (base=3)             | gelatinosa (base=8)           | dura (base=4)                |
| :---------------- | :--------------------------- | :---------------------------- | :--------------------------- |
| **miopia**        | $(2+1)/(3+2) = \mathbf{3/5}$ | $(4+1)/(8+2) = \mathbf{5/10}$ | $(2+1)/(4+2) = \mathbf{3/6}$ |
| **hipermetropia** | $(1+1)/(3+2) = \mathbf{2/5}$ | $(4+1)/(8+2) = \mathbf{5/10}$ | $(2+1)/(4+2) = \mathbf{3/6}$ |

- **Astigmatismo** ($|V| = 2$: não, sim)

| Astigmatismo | nenhuma (base=3)             | gelatinosa (base=8)           | dura (base=4)                |
| :----------- | :--------------------------- | :---------------------------- | :--------------------------- |
| **não**      | $(2+1)/(3+2) = \mathbf{3/5}$ | $(5+1)/(8+2) = \mathbf{6/10}$ | $(1+1)/(4+2) = \mathbf{2/6}$ |
| **sim**      | $(1+1)/(3+2) = \mathbf{2/5}$ | $(3+1)/(8+2) = \mathbf{4/10}$ | $(3+1)/(4+2) = \mathbf{4/6}$ |

- **Taxa lacrimal** ($|V| = 2$: reduzida, normal)

| Taxa Lacrimal | nenhuma (base=3)             | gelatinosa (base=8)           | dura (base=4)                |
| :------------ | :--------------------------- | :---------------------------- | :--------------------------- |
| **reduzida**  | $(3+1)/(3+2) = \mathbf{4/5}$ | $(2+1)/(8+2) = \mathbf{3/10}$ | $(0+1)/(4+2) = \mathbf{1/6}$ |
| **normal**    | $(0+1)/(3+2) = \mathbf{1/5}$ | $(6+1)/(8+2) = \mathbf{7/10}$ | $(4+1)/(4+2) = \mathbf{5/6}$ |

---

### **Exercício 2: Classificação do Novo Paciente via Naive Bayes**

**Paciente:** `Idade = infantil`, `Diagnóstico = hipermetropia`, `Astigmatismo = não`, `Taxa lacrimal = reduzida`.

Calculamos o _score_ não normalizado para cada classe multiplicando a _prior_ pelas condicionais do paciente:

- **Score(nenhuma)** $= P(\text{nenhuma}) \times P(\text{inf}|\text{nen.}) \times P(\text{hip}|\text{nen.}) \times P(\text{não}|\text{nen.}) \times P(\text{red}|\text{nen.})$
  $= \frac{4}{18} \times \frac{2}{6} \times \frac{2}{5} \times \frac{3}{5} \times \frac{4}{5} \approx \mathbf{0,01422}$

- **Score(gelatinosa)** $= P(\text{gel}) \times P(\text{inf}|\text{gel.}) \times P(\text{hip}|\text{gel.}) \times P(\text{não}|\text{gel.}) \times P(\text{red}|\text{gel.})$
  $= \frac{9}{18} \times \frac{3}{11} \times \frac{5}{10} \times \frac{6}{10} \times \frac{3}{10} \approx \mathbf{0,01227}$

- **Score(dura)** $= P(\text{dura}) \times P(\text{inf}|\text{dura}) \times P(\text{hip}|\text{dura}) \times P(\text{não}|\text{dura}) \times P(\text{red}|\text{dura})$
  $= \frac{5}{18} \times \frac{2}{7} \times \frac{3}{6} \times \frac{2}{6} \times \frac{1}{6} \approx \mathbf{0,00220}$

**Normalização:**
Soma dos scores $= 0,01422 + 0,01227 + 0,00220 = \mathbf{0,02869}$

- $P(\text{nenhuma}) = 0,01422 / 0,02869 = \mathbf{49,56\%}$
- $P(\text{gelatinosa}) = 0,01227 / 0,02869 = \mathbf{42,77\%}$
- $P(\text{dura}) = 0,00220 / 0,02869 = \mathbf{7,67\%}$

**Previsão:** A classe com maior probabilidade é **Nenhuma**.

---

### **Exercício 3: Estrutura e Tabelas da Rede Bayesiana**

**Estrutura da Rede:**

1.  **Nó raiz:** `Classe (Tipo de lente)`
2.  **Nós dependentes apenas da classe:** `Diagnóstico`, `Astigmatismo`, `Idade`.
3.  **Nó com dependência dupla:** `Taxa Lacrimal` depende de `Classe` **E** de `Idade`.

As tabelas para _Classe_, _Diagnóstico_, _Astigmatismo_ e _Idade_ são **exatamente as mesmas** calculadas no Exercício 1. A única tabela nova que precisamos calcular é a probabilidade conjunta $P(\text{Taxa Lacrimal} | \text{Classe, Idade})$, aplicando $\alpha=1$ (com $|V|=2$ para a taxa lacrimal).

**Tabela de Probabilidade:** $P(\text{Taxa Lacrimal} | \text{Classe, Idade})$

| Classe         | Idade       | Contagens Brutas (Red / Nor) |      $P(\text{reduzida}      |    \text{Classe, Idade})$    | $P(\text{normal} | \text{Classe, Idade})$ |
| :------------- | :---------- | :--------------------------: | :--------------------------: | :--------------------------: | ---------------- | ---------------------- |
| **nenhuma**    | infantil    |            1 / 0             | $(1+1)/(1+2) = \mathbf{2/3}$ | $(0+1)/(1+2) = \mathbf{1/3}$ |
| **nenhuma**    | adolescente |            1 / 0             | $(1+1)/(1+2) = \mathbf{2/3}$ | $(0+1)/(1+2) = \mathbf{1/3}$ |
| **nenhuma**    | adulto      |            1 / 0             | $(1+1)/(1+2) = \mathbf{2/3}$ | $(0+1)/(1+2) = \mathbf{1/3}$ |
| **gelatinosa** | infantil    |            0 / 2             | $(0+1)/(2+2) = \mathbf{1/4}$ | $(2+1)/(2+2) = \mathbf{3/4}$ |
| **gelatinosa** | adolescente |            2 / 0             | $(2+1)/(2+2) = \mathbf{3/4}$ | $(0+1)/(2+2) = \mathbf{1/4}$ |
| **gelatinosa** | adulto      |            0 / 4             | $(0+1)/(4+2) = \mathbf{1/6}$ | $(4+1)/(4+2) = \mathbf{5/6}$ |
| **dura**       | infantil    |            0 / 1             | $(0+1)/(1+2) = \mathbf{1/3}$ | $(1+1)/(1+2) = \mathbf{2/3}$ |
| **dura**       | adolescente |            0 / 2             | $(0+1)/(2+2) = \mathbf{1/4}$ | $(2+1)/(2+2) = \mathbf{3/4}$ |
| **dura**       | adulto      |            0 / 1             | $(0+1)/(1+2) = \mathbf{1/3}$ | $(1+1)/(1+2) = \mathbf{2/3}$ |

---

### **Exercício 4: Reclassificação via Rede Bayesiana**

Vamos reclassificar o mesmo paciente do Exercício 2, mas agora substituindo a probabilidade isolada $P(\text{Taxa Lacrimal} | \text{Classe})$ pela probabilidade condicionada $P(\text{Taxa Lacrimal} | \text{Classe, Idade})$ calculada na rede bayesiana.

O paciente possui: `Idade = infantil` e `Taxa = reduzida`.

- **Score(nenhuma)** $= P(\text{nenhuma}) \times P(\text{inf}|\text{nen.}) \times P(\text{hip}|\text{nen.}) \times P(\text{não}|\text{nen.}) \times \mathbf{P(\text{red}|\text{nen., inf})}$
  $= \frac{4}{18} \times \frac{2}{6} \times \frac{2}{5} \times \frac{3}{5} \times \mathbf{\frac{2}{3}} \approx \mathbf{0,01185}$

- **Score(gelatinosa)** $= P(\text{gel}) \times P(\text{inf}|\text{gel.}) \times P(\text{hip}|\text{gel.}) \times P(\text{não}|\text{gel.}) \times \mathbf{P(\text{red}|\text{gel., inf})}$
  $= \frac{9}{18} \times \frac{3}{11} \times \frac{5}{10} \times \frac{6}{10} \times \mathbf{\frac{1}{4}} \approx \mathbf{0,01023}$

- **Score(dura)** $= P(\text{dura}) \times P(\text{inf}|\text{dura}) \times P(\text{hip}|\text{dura}) \times P(\text{não}|\text{dura}) \times \mathbf{P(\text{red}|\text{dura, inf})}$
  $= \frac{5}{18} \times \frac{2}{7} \times \frac{3}{6} \times \frac{2}{6} \times \mathbf{\frac{1}{3}} \approx \mathbf{0,00441}$

**Normalização:**
Soma dos scores $= 0,01185 + 0,01023 + 0,00441 = \mathbf{0,02649}$

- $P(\text{nenhuma}) = 0,01185 / 0,02649 = \mathbf{44,73\%}$
- $P(\text{gelatinosa}) = 0,01023 / 0,02649 = \mathbf{38,62\%}$
- $P(\text{dura}) = 0,00441 / 0,02649 = \mathbf{16,65\%}$

**Previsão final:** Apesar das probabilidades sofrerem uma leve alteração devido à inclusão da dependência condicional extra, a classe prevista permanece como **Nenhuma**.
