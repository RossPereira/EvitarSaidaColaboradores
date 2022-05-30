# Problema de Negócio e Solução:

**Problema de Negócio:** Estamos com vários funcionários se desligando da empresa, isso aumentou consideravelmente nossos custos com contratação e treinamento, precisamos de uma solução para poder evitar a saída dos antigos funcionários e prologar a permanencia dos novos.

**Solução proposta:** Criar um Modelo de Machine Learning que passados os dados possa fazer um previsão se o colaborador tem potêncial de sair da empresa ou não

# Bibliotecas Usadas:

* Scikit learn
* Pandas
* Numpy
* Seaborn
* MatPlotLib

# Construção:

Foram levantadas algumas hipóteses junto ao time de negócios para entendermos melhor o evento a ser modelado, os dados necessários então foram reunidos, após o levantamento dos dados foram feitas as análises descritivas e estatísticas.

Antes de realizar a análise exploratória dos dados, foi feito um pré-processamento rápido para verificar valores nulos e vazios para que os mesmo não influenciassem a exploração dos dados.

Posteriormente foi feita a análise exploratória propriamente dita, para validarmos ou invalidarmos as hipóteses levantadas.

Na fase de pré-processamento além da verificação de valores nulos e vazios que ja tinha sido feita, foram removidas algumas colunas como idade ou número do colaboradores que foram julgadas como não relevates para o modelo assim evitando o ruído no treino, além disso foi aplicada uma máscara nas colunas de Attrition (nossa feature alvo de previsão) e OverTime onde as mesmas possuíam valores Yes e No que foram substituídos por 1 e 0 respectivamente, foram separadas as features categoricas(textos) das numéricas, como todas elas não possuiam ordem (não eram ordinais) foi aplicado o OneHotEnconder em todas. Foi feita também a padronização dos dados.

Na base possuíamos muito mais registros de funcionários que permaneceram na empresa do que funcionários que sairam, foram feitos testes de geração de modelos com os dados desbalanceados mas a precision e o recall ficaram abaixo dos 50% o que removeria o sentido de implementar um modelo de previsão, foi feita então a geração de novos registros sintéticos da classe minoritária com o algoritmo SMOTE (que utiliza distância euclidiana para gerar novos registros através dos registros existentes da classe minoritária).

Para construção do modelo de classificação, foi criado um pipeline com os principais algoritmos de classificação, utlizando Cross-Validation e tendo como métricas accuracy, recall e precision para testar vários algoritmos em paralelo e escolher 2 com melhores resultados para prosseguir. Foram observados melhores resultados com Random Forest e Logistic Regression. Selecionado os algoritmos foi aplicada a técnica de Random Search para otimizar os hiperparâmetros. Para a seleção das features separadamente foram feitas duas funções com SelectKBest para cada valor de K comparando os resultados do erro médio absoluto. Para gerar os modelos finais a base foi dividida utilizando o método padrão Houldout, separei 20% dos dados para teste sendo que dos 80% de treino 20% foram selecionados para validação para evitarmos vazamento de dados.

Após a construção e avaliação dos resultados o classificador Random Forest foi escolhido para ser salvo e utlizado em produção com as seguintes métricas (Accuracy: 85%, Precision: 78% para classe de colaboradores que saíram(1) e 95% para os que ficaram(0) e Recall: 96% para a classe 1 e 74% para a classe 0.
