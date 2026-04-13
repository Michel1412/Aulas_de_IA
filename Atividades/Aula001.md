Relatório de Otimização: Previsão de Clima (KNN e Árvore de Decisão)

1. O que deu errado na versão original?

Na versão original construída em sala de aula, o modelo utilizou as três variáveis disponíveis: latitude, longitude e altitude_m com K=5. Com essa configuração, a acurácia foi muito baixa (60% no KNN e 40% na Árvore de Decisão). O que deu errado foi a inclusão de features que inseriram "ruído" na análise espacial do algoritmo. Como o KNN calcula a distância matemática entre os pontos para achar os "vizinhos", colocar uma variável que não tem impacto direto no clima acabou distorcendo a proximidade real das cidades que possuem temperaturas semelhantes.

2. Tabela de Simulações (Simulação A - Alteração de Features)

O algoritmo KNN com K=3 e um split de 20% para teste (5 cidades), testamos a remoção de variáveis para analisar o impacto isolado de cada uma.

Cenário Base: Features (latitude, longitude, altitude_m)

Acurácia obtida: 0.60 (60%)

Cenário 1 (Sem Altitude / Apenas BC): Features (latitude, longitude)

Acurácia obtida: 0.80 (80%)

Observação: O modelo melhorou, errando apenas a previsão da cidade de Fortaleza.

Cenário 2 (Sem Longitude / Apenas BD): Features (latitude, altitude_m)

Acurácia obtida: 1.00 (100%)

Observação: O modelo atingiu a perfeição no conjunto de teste, acertando todas as classes (frio, quente e ameno).

3. Tabela de Simulações (Simulação B - Ajuste do Parâmetro K no KNN)

Retornamos as três features originais (latitude, longitude, altitude_m) para testar o impacto de mudar apenas o número de vizinhos analisados (K) no algoritmo KNN.

Cenário 3 (Ajuste para K=1): \* Acurácia obtida: 0.80 (80%)

Cenário 4 (Ajuste para K=2): \* Acurácia obtida: 0.80 (80%)

Observação conjunta: Reduzir o valor de K de 3 para 1 ou 2 fez o modelo subir de 60% para 80% de acurácia. Em ambos os casos, o único erro foi classificar Fortaleza como "ameno" (quando o real é "quente").

4. Conclusão dos Motivos

A melhor configuração geral alcançada foi a do Cenário 2, utilizando apenas a Latitude e a Altitude, que elevou a precisão do modelo para 100%. Isso ocorreu porque a temperatura de uma cidade é fortemente ditada pela sua distância em relação à Linha do Equador e sua elevação, enquanto a Longitude não possui correlação direta, gerando apenas ruído matemático no cálculo de distâncias do KNN.

Os testes da Simulação B (K=1 e K=2) confirmam essa tese. Ao reduzir o K, nós forçamos o modelo a olhar apenas para o vizinho mais próximo, ignorando vizinhos mais distantes que estavam sendo "trazidos" para o cálculo por causa da distorção gerada pela Longitude. Isso melhorou a acurácia para 80%, mas provou que limpar os dados de entrada (removendo features ruins, como no Cenário 2) é uma estratégia muito mais eficaz do que tentar compensar o ruído alterando os hiperparâmetros do modelo.
