1. Conceitual – Importância da Preparação

A etapa de preparação de dados visa melhorar a qualidade dos dados brutos, o que possibilita a indução de modelos preditivos melhores e reduz a complexidade computacional. Como os modelos de Machine Learning dependem integralmente das informações contidas nos atributos para "aprender", entregar a eles dados repletos de ruídos, valores ausentes ou em formatos inadequados fará com que até o algoritmo mais avançado tenha um desempenho ruim.

2. Valores Ausentes

a) As possíveis soluções para resolver esse problema são: remoção das instâncias que contêm valores ausentes, preenchimento manual dos valores, ou o preenchimento automático (atribuindo a média, moda, mediana, valor constante ou até usando modelos preditivos para estimá-los).

b) Remover os registros é inadequado justamente quando a quantidade de exemplos for muito grande (como neste cenário de 25%), pois faria com que o modelo perdesse um volume gigantesco de informações que poderiam ser vitais para o treinamento e compreensão de padrões pelo algoritmo.

3. Normalização vs Padronização

Padronização (Z-score): Utilizada para dados normalmente distribuídos, aplica-se a normalização baseada na média e no desvio padrão do atributo.

Normalização (Min-Max): Também chamada de reescala, aplica um ajuste linear baseado no menor e no maior valor da base, realocando os limites do atributo (frequentemente para intervalos entre [0, 1]).

Recomendação: A Padronização costuma ser preferida em algoritmos baseados em descida de gradiente ou cálculos de distância puras (Redes Neurais, Regressão Logística), enquanto a Normalização Min-Max é bastante utilizada quando os dados não formam uma distribuição normal ou exigem escalas rigidamente fixadas (ex: dados de imagens para Computer Vision e K-NN).

4. Dados Categóricos

a) Essa variável simbólica deve ser convertida em um formato numérico através da criação de "pseudoatributos" binários. Utilizando a técnica de Codificação One-hot, a variável clima passaria a ser representada por 3 colunas distintas de presença ou ausência (ex: "é_sol", "é_chuva", "é_nublado").

b) Utilizar uma conversão de codificação numérica simples de ordem cria uma falsa relação de hierarquia. O algoritmo interpretaria, erroneamente, que a classe 2 ("nublado") tem um peso matemático maior que a 1 ("chuva"), afetando os cálculos preditivos.

5. Overfitting e Separação de Dados

Treino: Conjunto contendo exemplos usados estritamente para treinar (induzir) o modelo para que ele aprenda a partir deles.

Validação: Uma subdivisão feita a partir dos dados de treino para ajudar de forma contínua a orientar a escolha dos hiperparâmetros sem enviesar a avaliação final.

Teste: Conjunto de dados isolado no início e apresentado ao modelo apenas na etapa final para estimar de maneira realista o seu desempenho em casos que ele nunca viu antes.

O que acontece: Se utilizar o teste durante o treinamento ocorre o fenômeno do vazamento de dados. O modelo simplesmente vai "decorar" as instâncias, resultando no Overfitting (sobreajuste), passando no teste com notas altas, mas falhando miseravelmente no mundo real por incapacidade de generalização.

6. Balanceamento de Classes

a) Conjuntos de dados desbalanceados afetam o desempenho da maioria dos algoritmos, pois o modelo fatalmente irá favorecer a classificação e chutar para a classe majoritária (A). Como consequência, o modelo gera uma falsa ilusão de alta precisão no geral, porém tem baixo ou nulo desempenho ao classificar a classe (B).

<br>

b) Duas técnicas de reamostragem clássicas para evitar isso são:

Subamostragem (undersampling): Que remove instâncias pertencentes à classe majoritária.

Sobreamostragem (oversampling): Que adiciona/sintetiza novas instâncias para a classe minoritária, como o SMOTE e o ADASYN.

7. Detecção de Outliers

- Outliers são instâncias ruidosas de valores atípicos em relação aos demais; ou seja, que se encontram isolados e muito fora da distribuição estatística esperada para aqueles dados.

- Pode ser prejudicial quando o valor atípico não é fruto de um erro no processo de aquisição, mas sim uma representação legítima de algo vital que o modelo precisa saber identificar. (Ex: O pico de saque numa conta causado por uma fraude).

- Utilizando medidas de dispersão, como a variância e o desvio padrão atrelados à média, que possibilita criar intervalos e isolar matematicamente as instâncias que estão dispersas com magnitude atípica.

8. Feature Engineering

- O processo de extração de atributos foca em criar as melhores características e definir a representação adequada dos objetos. Algoritmos matemáticos não interpretam formatos crús de calendário com facilidade. Transformar data_nascimento em idade transforma uma estampa simbólica num vetor numérico contínuo e escalonável de magnitude simples, facilitando que o algoritmo capte ligações diretas baseadas na passagem do tempo.

9. Pipeline de Pré-processamento

Para solucionar a sequência apresentada no cenário, a estrutura ideal do Pipeline seguiria os seguintes estágios em ordem:

Limpeza (Valores ausentes): Tratar imediatamente os valores nulos utilizando heurísticas de preenchimento automático contínuo (ex: usar média/mediana para preencher atributos numéricos) visando não apagar as instâncias do banco.

Transformação (Dados categóricos): Converter os tipos de atributos descritivos e categóricos em representações numéricas usando técnicas como One-hot Encoding (criando colunas binárias novas) ou codificação ordinal para que os algoritmos consigam lê-los.

Transformação Escalar (Variáveis com escalas diferentes): Executar métodos de reescala (Normalização Min-max ou Padronização Z-score) para garantir que todas as colunas numéricas variem na mesma ordem de grandeza e uma não domine a outra por pura diferença matemática.

Balanceamento por Amostragem (Classes desbalanceadas): Como a estrutura final agora está limpa, converta e padronizada, aplicar técnicas de inserção artificial sintética (ex: SMOTE) apenas nos dados separados de treinamento para igualar a distribuição de quem é classe majoritária com quem é minoritária.

10. Estudo de Caso Aplicado

Para preparar a base bancária e treinar corretamente este classificador financeiro, é imperativo:

Limpeza Inicial e Limites: Checar a presença de valores ausentes (ex: campos vazios em renda_mensal) e tratá-los preenchendo automaticamente através do método de imputação da média ou moda. Inspecionar ruídos (ex: dependentes = "200") e cortá-los via desvio padrão.

Conversão de Dados: O estado_civil representa valor categórico e precisa passar por transformação para numérico via uso de One-hot Encoding, ganhando colunas falsas de "é_casado", "é_solteiro". O histórico_de_inadimplência também pode ser categorizado numéricamente.

Normalização (Escalonamento): Como os atributos possuem magnitudes totalmente distintas (a renda_mensal possui variações na casa dos milhares enquanto número_de_dependentes e idade são baixos de dezenas e unidades matemáticas curtas), esse contraste numérico dominará e enganará o cálculo do modelo. Executar uma padronização nestes numéricos para reescalá-los no modelo.

Reamostragem e Separação: Separar todo o dataset limpo em pacotes para Teste e Treinamento isolados. No pacote do treino, conferir a proporção do atributo-meta; caso existam muito mais pessoas aprovadas do que rejeitadas (classes desbalanceadas), executar o SMOTE para inflar os exemplos minoritários artificialmente antes de efetuar o aprendizado de máquina em si.
