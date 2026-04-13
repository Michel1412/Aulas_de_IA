**Cálculo das Distâncias Euclidianas:**
A fórmula da distância euclidiana é $d_E(x_1, x_2) = \sqrt{\sum_{i=1}^{d} (x1_i - x2_i)^2}$.

- **P1 (2.0, 9.0) - Vermelho:** $d = \sqrt{(4.2 - 2.0)^2 + (9.0 - 9.0)^2} = \sqrt{2.2^2} = 2.20$
- **P2 (6.5, 6.0) - Azul:** $d = \sqrt{(4.2 - 6.5)^2 + (9.0 - 6.0)^2} = \sqrt{(-2.3)^2 + 3.0^2} = \sqrt{5.29 + 9.00} \approx 3.78$
- **P3 (6.0, 2.5) - Azul:** $d = \sqrt{(4.2 - 6.0)^2 + (9.0 - 2.5)^2} = \sqrt{(-1.8)^2 + 6.5^2} = \sqrt{3.24 + 42.25} \approx 6.74$
- **P4 (5.5, 8.0) - Azul:** $d = \sqrt{(4.2 - 5.5)^2 + (9.0 - 8.0)^2} = \sqrt{(-1.3)^2 + 1.0^2} = \sqrt{1.69 + 1.00} \approx 1.64$
- **P5 (5.0, 5.0) - Azul:** $d = \sqrt{(4.2 - 5.0)^2 + (9.0 - 5.0)^2} = \sqrt{(-0.8)^2 + 4.0^2} = \sqrt{0.64 + 16.00} \approx 4.08$
- **P6 (4.0, 10.0) - Vermelho:** $d = \sqrt{(4.2 - 4.0)^2 + (9.0 - 10.0)^2} = \sqrt{0.2^2 + (-1.0)^2} = \sqrt{0.04 + 1.00} \approx 1.02$
- **P7 (7.0, 4.0) - Azul:** $d = \sqrt{(4.2 - 7.0)^2 + (9.0 - 4.0)^2} = \sqrt{(-2.8)^2 + 5.0^2} = \sqrt{7.84 + 25.00} \approx 5.73$
- **P8 (4.0, 7.5) - Azul:** $d = \sqrt{(4.2 - 4.0)^2 + (9.0 - 7.5)^2} = \sqrt{0.2^2 + 1.5^2} = \sqrt{0.04 + 2.25} \approx 1.51$
- **P9 (3.9, 5.8) - Vermelho:** $d = \sqrt{(4.2 - 3.9)^2 + (9.0 - 5.8)^2} = \sqrt{0.3^2 + 3.2^2} = \sqrt{0.09 + 10.24} \approx 3.21$
- **P10 (4.5, 7.3) - Azul:** $d = \sqrt{(4.2 - 4.5)^2 + (9.0 - 7.3)^2} = \sqrt{(-0.3)^2 + 1.7^2} = \sqrt{0.09 + 2.89} \approx 1.73$

---

### Exercício 1: k-NN com $k=3$

Para o algoritmo k-NN tradicional, selecionamos as instâncias com as menores distâncias e classificamos por votação majoritária.

**Os $3$ vizinhos mais próximos de $x_q(4.2, 9.0)$ são:**

1.  **P6**: Distância $\approx 1.02$ (Classe: **Vermelho**)
2.  **P8**: Distância $\approx 1.51$ (Classe: **Azul**)
3.  **P4**: Distância $\approx 1.64$ (Classe: **Azul**)

**Votação:**

- Votos para a classe **Vermelho**: 1
- Votos para a classe **Azul**: 2

**Resposta:** O algoritmo k-NN classifica o exemplo como **Azul**.

---

### Exercício 2: k-NN Ponderado pela Distância $(1/dist)$

Neste método, cada vizinho contribui com um peso inversamente proporcional à sua distância ($w_i = 1 / d(x_i, x_q)$). Somamos os pesos de cada classe para determinar a resposta.

**Cálculo dos Pesos para os $3$ vizinhos mais próximos:**

- **P6 (Vermelho):** $w_1 = 1 / 1.02 \approx 0.98$
- **P8 (Azul):** $w_2 = 1 / 1.51 \approx 0.66$
- **P4 (Azul):** $w_3 = 1 / 1.64 \approx 0.61$

**Pontuação por Classe:**

- Score **Vermelho**: $0.98$
- Score **Azul**: $0.66 + 0.61 = 1.27$

**Resposta:** Como o score da classe Azul é maior ($1.27 > 0.98$), o classificador k-NN ponderado também classifica o exemplo como **Azul**.

---

### Exercício 3: Algoritmo Nearest Centroid

O algoritmo _Nearest Centroid_ requer que calculemos o ponto médio (centroide) para todos os membros de cada classe e classifiquemos a nova instância com base no centroide mais próximo.

**1. Cálculo do Centroide da Classe Vermelho ($C_V$):**
Pontos Vermelhos: $P1(2, 9), P6(4, 10), P9(3.9, 5.8)$.
$$C_V = \left(\frac{2 + 4 + 3.9}{3}, \frac{9 + 10 + 5.8}{3}\right) = \left(\frac{9.9}{3}, \frac{24.8}{3}\right) \approx (3.30, 8.27)$$

**2. Cálculo do Centroide da Classe Azul ($C_A$):**
Pontos Azuis: $P2(6.5, 6), P3(6, 2.5), P4(5.5, 8), P5(5, 5), P7(7, 4), P8(4, 7.5), P10(4.5, 7.3)$.
$$C_A = \left(\frac{6.5+6+5.5+5+7+4+4.5}{7}, \frac{6+2.5+8+5+4+7.5+7.3}{7}\right)$$
$$C_A = \left(\frac{38.5}{7}, \frac{40.3}{7}\right) \approx (5.50, 5.76)$$

**3. Distância do exemplo $x_q(4.2, 9.0)$ para os Centroides:**

- **Distância para $C_V$**:
  $$d(x_q, C_V) = \sqrt{(4.2 - 3.30)^2 + (9.0 - 8.27)^2} = \sqrt{0.90^2 + 0.73^2} = \sqrt{0.81 + 0.5329} \approx \sqrt{1.34} \approx 1.16$$
- **Distância para $C_A$**:
  $$d(x_q, C_A) = \sqrt{(4.2 - 5.50)^2 + (9.0 - 5.76)^2} = \sqrt{(-1.3)^2 + 3.24^2} = \sqrt{1.69 + 10.4976} \approx \sqrt{12.19} \approx 3.49$$

**Resposta:** Como a distância para o centroide da classe Vermelho ($1.16$) é consideravelmente menor que a distância para o centroide da classe Azul ($3.49$), o algoritmo _Nearest Centroid_ classifica o exemplo como **Vermelho**.
