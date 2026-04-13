### Análise Inicial do Conjunto de Treinamento

Analisando a tabela fornecida, o conjunto completo $S$ possui 15 exemplos. A distribuição da classe alvo ("Tipo de lente") é:

- **nenhuma:** 3 exemplos
- **gelatinosa:** 8 exemplos
- **dura:** 4 exemplos

A entropia inicial do conjunto $S$ é calculada como:
$$Entropia(S) = - \left(\frac{3}{15}\right)\log_2\left(\frac{3}{15}\right) - \left(\frac{8}{15}\right)\log_2\left(\frac{8}{15}\right) - \left(\frac{4}{15}\right)\log_2\left(\frac{4}{15}\right)$$
$$Entropia(S) = - (0.2 \times -2.32) - (0.53 \times -0.91) - (0.27 \times -1.91)$$
$$Entropia(S) = 0.46 + 0.48 + 0.51 = 1.46$$

---

### a) O atributo Taxa Lacrimal será escolhido como raiz. Mostre os cálculos que justificam.

Para justificar essa escolha, precisamos calcular o Ganho de Informação ($IG$) de todos os atributos e provar que "Taxa lacrimal" possui o maior valor.

**1. Atributo: Taxa Lacrimal**

- **reduzida** (5 exemplos: 3 nenhuma, 2 gelatinosa, 0 dura):
  $$Entropia(S_{reduzida}) = - \left(\frac{3}{5}\right)\log_2\left(\frac{3}{5}\right) - \left(\frac{2}{5}\right)\log_2\left(\frac{2}{5}\right) = 0.97$$
- **normal** (10 exemplos: 0 nenhuma, 6 gelatinosa, 4 dura):
  $$Entropia(S_{normal}) = - \left(\frac{6}{10}\right)\log_2\left(\frac{6}{10}\right) - \left(\frac{4}{10}\right)\log_2\left(\frac{4}{10}\right) = 0.97$$
- **Ganho de Informação:**
  $$IG(Taxa\_Lacrimal) = 1.46 - \left[ \left(\frac{5}{15} \times 0.97\right) + \left(\frac{10}{15} \times 0.97\right) \right] = 1.46 - 0.97 = \mathbf{0.49}$$

**2. Atributo: Idade**

- **jovem** (4 exemplos: 1 nenhuma, 2 gelatinosa, 1 dura) $\rightarrow Entropia = 1.50$
- **pré-presbiopia** (5 exemplos: 1 nenhuma, 2 gelatinosa, 2 dura) $\rightarrow Entropia = 1.52$
- **presbiopia** (6 exemplos: 1 nenhuma, 4 gelatinosa, 1 dura) $\rightarrow Entropia = 1.25$
- $$IG(Idade) = 1.46 - \left[ \left(\frac{4}{15} \times 1.50\right) + \left(\frac{5}{15} \times 1.52\right) + \left(\frac{6}{15} \times 1.25\right) \right] = 1.46 - 1.41 = \mathbf{0.05}$$

**3. Atributo: Diagnóstico**

- **miopia** (8 exemplos: 2 nenhuma, 4 gelatinosa, 2 dura) $\rightarrow Entropia = 1.50$
- **hipermetropia** (7 exemplos: 1 nenhuma, 4 gelatinosa, 2 dura) $\rightarrow Entropia = 1.38$
- $$IG(Diagnostico) = 1.46 - \left[ \left(\frac{8}{15} \times 1.50\right) + \left(\frac{7}{15} \times 1.38\right) \right] = 1.46 - 1.44 = \mathbf{0.02}$$

**4. Atributo: Astigmatismo**

- **não** (8 exemplos: 2 nenhuma, 5 gelatinosa, 1 dura) $\rightarrow Entropia = 1.30$
- **sim** (7 exemplos: 1 nenhuma, 3 gelatinosa, 3 dura) $\rightarrow Entropia = 1.44$
- $$IG(Astigmatismo) = 1.46 - \left[ \left(\frac{8}{15} \times 1.30\right) + \left(\frac{7}{15} \times 1.44\right) \right] = 1.46 - 1.36 = \mathbf{0.10}$$

**Conclusão da letra a:** O atributo **Taxa lacrimal** é escolhido como raiz porque apresenta o maior Ganho de Informação ($0.49$) comparado aos demais atributos.

---

### b) A partir da raiz, desenvolva o ramo Taxa lacrimal = reduzida

Filtrando o conjunto de treinamento apenas para os casos onde `Taxa lacrimal = reduzida`, ficamos com um subconjunto ($S_{red}$) de 5 exemplos.

- Distribuição da classe neste ramo: **nenhuma** (3), **gelatinosa** (2), **dura** (0).
- A entropia deste nó, como calculado anteriormente, é $Entropia(S_{red}) = 0.97$.

Agora, calculamos o Ganho de Informação para os atributos restantes (`Idade`, `Diagnóstico`, `Astigmatismo`) considerando apenas esses 5 exemplos.

**1. Avaliando 'Idade' no ramo 'reduzida'**

- **jovem** (1 ex: 1 nenhuma) $\rightarrow Entropia = 0$ (Puro)
- **presbiopia** (1 ex: 1 nenhuma) $\rightarrow Entropia = 0$ (Puro)
- **pré-presbiopia** (3 ex: 1 nenhuma, 2 gelatinosa) $\rightarrow Entropia = - (1/3)\log_2(1/3) - (2/3)\log_2(2/3) = 0.92$
- $$IG(Idade|red) = 0.97 - \left[ \left(\frac{1}{5} \times 0\right) + \left(\frac{1}{5} \times 0\right) + \left(\frac{3}{5} \times 0.92\right) \right] = 0.97 - 0.55 = \mathbf{0.42}$$

**2. Avaliando 'Diagnóstico' no ramo 'reduzida'**

- **miopia** (3 ex: 2 nenhuma, 1 gelatinosa) $\rightarrow Entropia = 0.92$
- **hipermetropia** (2 ex: 1 nenhuma, 1 gelatinosa) $\rightarrow Entropia = 1.00$
- $$IG(Diagnostico|red) = 0.97 - \left[ \left(\frac{3}{5} \times 0.92\right) + \left(\frac{2}{5} \times 1.00\right) \right] = 0.97 - 0.95 = \mathbf{0.02}$$

**3. Avaliando 'Astigmatismo' no ramo 'reduzida'**

- **não** (4 ex: 2 nenhuma, 2 gelatinosa) $\rightarrow Entropia = 1.00$
- **sim** (1 ex: 1 nenhuma) $\rightarrow Entropia = 0$ (Puro)
- $$IG(Astigmatismo|red) = 0.97 - \left[ \left(\frac{4}{5} \times 1.00\right) + \left(\frac{1}{5} \times 0\right) \right] = 0.97 - 0.80 = \mathbf{0.17}$$

**Desenvolvimento do Ramo:**
O atributo **Idade** possui o maior Ganho de Informação ($0.42$) neste subconjunto e passa a ser o próximo nó de decisão. A ramificação fica assim:

1.  **Taxa lacrimal = reduzida**
    - **Idade = jovem:** Retorna a folha **nenhuma** (Puro).
    - **Idade = presbiopia:** Retorna a folha **nenhuma** (Puro).
    - **Idade = pré-presbiopia:** Este ramo ainda está impuro (1 nenhuma, 2 gelatinosa) e precisaria de uma nova divisão utilizando os atributos restantes (Diagnóstico ou Astigmatismo).
