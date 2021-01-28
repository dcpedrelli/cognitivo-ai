# Desafio Cognitivo.ai

Abaixo seguem as perguntas a serem respondidas no desafio:

## a. Como foi a definição da sua estratégia de modelagem?

Inicialmente, os modelo estavam com um $R^2$ muito baixo, por volta de 0.5, independentemente do modelo utilizado. Testei diversas abordagens para os dados do AirBnB presentes no Kaggle, mas nenhuma performava bem nos dados que eu possuo, que incluem um período de uns dois meses. Foi necessário então uma busca por melhor qualidade dos dados. Para isso fiz um merge dos listings com o calendar. No entanto, esse merge gerou uma quantidade muito grande de dados para o poder computacional, o que impossibilitou fazer um GridSearch em modelos como Random Forest por exemplo. No entanto, com esses dados fui capaz de chegar num score de 0.69, sem utilizar qualquer otimização. Por conta desse custo computacional decidi utilizar o LightGBM, que é um modelo mais leve, capaz de lidar com um grande volumen de dados, e utiliza técnicas de boosting para otimização do modelo.

 ## b. Como foi definida a função de custo utilizada?

A função de custo pode ser definida com metrics no LGBMRegressor. No caso escolhi a métrica de Mean abolute error. Apesar de ser uma métrica que não faz diferença entre pequena e grande diferenças em termos de punição, é fácil explicar para algum cliente o que a mesma significa. Pois se entende que o erro médio absoluto é um valor para mais ou menos da previsão feita.

## c. Qual foi o critério utilizado na seleção do modelo final?

Como comentei no item a, o LighGBM foi o único modelo que consegui fazer o teste de alguns parâmetros sem que isso tomasse muito custo computacional. Além do que, em relação à Random Forest por exemplo, conseguiu um $R^2$ maior, alé de não over-fitar.

## d. Qual foi o critério utilizado para validação do modelo?

Para validação do modelo fiz uma separação em dados de teste e treino. Em seguida, implementei um K-fold cross validation sobre os dados de treino, de modo que pudesse obter métricas que independessem do do split em treino e teste realizado. Depois fiz a predição do modelo para a base de treino e teste, e vi que o $R^2$ é similar para os dois caso, o que é bom, pois mostra que o modelo não está overfitando. 

### Por que escolheu utilizar este método?
Pois separa bem a análise entre treino e teste, além de mostrar com clareza se houve overfit, e se o modelo será bom ao ser aplicado em dados da vida real, ou seja, quando colocado em produção.

## e. Quais evidências você possui de que seu modelo é suficientemente bom?

Como discuti acima, todo um trabalho foi feito para selecionar as melhores variáveis, eliminar outliers, evidenciar correlações, escolher um modelo que suportasse optimização e que pudesse ser leve o suficiente para ser posto em produção, uma validação adequada foi realizada, tanto em separação da base em treino e teste, quanto na validação via cross validation sobre a base de treino. Com base nisso, estou satisfeito com o modelo, pois está performando sem overfit e apenas sobre dados de dois meses. Modelos com dados do ano, por exemplo, tem performado muito melhor, mas quando aplicado sobre os dados que trabalhei, performam muito mal. Assim, acredito que o modelo atual, ao encontrar dados anuais, pode ser ajustado para fornecer excelentes métricas.

Além do que foi feito, um Randomized Search ou Gri Search seria ideal para melhorar o modelo, mas a necessidade computacional elevou muito o tempo de processamento, e por conta disso mantive o modelo, apenas aumentando no número de árvores para 3000.