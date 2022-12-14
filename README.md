<br>

<p align="center">
  <img src="figures/santander.png" width="600" height="300" />
</p>

<h1 align="center"> Kaggle : Santander Customer Transaction Prediction  </h1>

![Badge concluded](http://img.shields.io/static/v1?label=STATUS&message=CONCLUDED&color=GREEN&style=for-the-badge)

## Descrição

  Este projeto tem como propósito estudar os dados fornecidos no problema "Santander Customer Transaction Prediction" hospedado no Kaggle no link:

```https://www.kaggle.com/c/santander-customer-transaction-prediction```

  A ideia é utilizar este problema para fazer uma análise preditiva, explorando diferentes modelos de machine learning, para o caso de classificação.

## O problema a ser analisado

Como a base de dados do **'Santander Customer Transaction Prediction'** tem o seguinte texto guia: 

   * "In this challenge, we invite Kagglers to help us identify which customers will make a specific transaction in the future, irrespective of the amount of money transacted. The data provided for this competition has the same structure as the real data we have available to solve this problem." *


Como temos um problema de classificação binária, e o próprio desafio sugere que a análise conste como uma previsão de que os clientes do banco realizem uma determinada operação no futuro, dadas as variáveis disponíveis no banco de dados do Santander, podemos formular um problema de negócio em que o banco estará oferecendo para o cliente um investimento de renda fixa do programa “Renda Mais”. Neste caso, os modelos de machine learning tem o objetivo de prever se um dado cliente irá participar do programa ou não.

Assim, as variáveis disponíveis poderiam ser interpretados como informações relacionadas a empréstimos realizados e pagos, pagamentos, idade, renda, situação civil e outras trasações e investimentos no banco.

Portanto, queremos criar um modelo de previsão que forneça o indicativo sobre quais clientes possuem maior probabilidade de fazer parte do programa "Renda Mais", de forma que os clientes classificados positivamente (1) receberão e-mails com propostas e propagandas referentes ao programa de investimento.

Para este caso, observa-se que falsos positivos (FP) não são um problema grave, uma vez que um cliente que seja classificado positivamente mas que não tenha interesse no programa poderá apenas ignorar ou recusar a oferta. Entretanto, o caso de falso negativo (FN) é considerado um problema grave, uma vez que o cliente classificado negativamente mas que tenha interesse em investir não recebera a oferta e, assim, o banco perde um cliente em potencial e não haverá lucros. A métrica de classificação de maior importância é o recall, de forma que maximizando o recall estaremos diminuindo o número de falsos negativos.
O recall é dado pela equação:


$$ recall = \frac{VP}{VP + FN}$$


O problema consta com a variável **ID_code** que é a identificação do cliente, a **target** que é o problema que estamos querendo resolver, isto é, é a variável do tipo classe (binária, 0 ou 1), e teremos um total de **200** features, identificadas como: var_0, var_1, ..., var_199.

Um detalhe é que a base de dados de teste não possui a coluna target, então não seria possível verificar as métricas do nosso modelo utilizando esses dados, portanto, o que será feito é dividir a base de treino em duas: uma efetivamente de treino e outra de validação, de forma a ter uma proporção 70% e 30%, respectivamente.
Além disso, a base de dados já esta bastante limpa, não sendo necessário fazer um trabalho arduo de pré-processamento dos dados.

## Resultados 

#### Primeiras observações 

Legenda para entendimento do problema: 
   *  ID_code :  identificação do cliente
   *  target: variável que queremos prever
   *  var_i ; i = 0, 1, ..., 199
   
observa-se que em ambos conjuntos de dados, treino e validacao, não há valores nulos. Também não será necessário fazer processo de cleaning e escalonamento, visto que as variáveis estão sem significado e as ordens de grandeza estão próximas. 

Temos um caso de classes desbalanceadas, sendo as contagens:
  - Treino:
      * classe 0 : 125972 contagens  
      * classe 1 : 14028 contagens
  - Validação:
      * classe 0 : 53930 contagens  
      * classe 1 : 6070 contagens

<p float="left" align="center">
  <img src="figures/target_count_teste.png" width="400" height="250"/>
  <img src="figures/target_count_validation.png" width="400" height="250"/>
</p>

As variáveis não são correlacionadas, de forma que não existe uma correlação acima de 0.5 entre as variáveis. Isto pode ser observado pela figura abaixo:

<p align="center">
  <img src="figures/correlacao.png" width="900" height="450" />
</p>


Quatro modelos de machine learning foram utilizados: ``Regressão Logística, Random Forest, Gradient Boosted Decision Tree (XGBoost) e Redes Neurais.``


Os modelos foram avaliados segundo a métrica **recall**. De forma que, resumidamente, obtivemos os seguintes resultados:

   -  **Random Forest**: 0.72
   - **Regressão Logistica**: 0.80
   - **GBDT**: 0.82
   - **Rede Neural**: 0.78    
   
Com isso, olhando apenas para o recall, a GBDT demonstrou possuir uma melhor performance, apesar de ter apresentado overfiting com o modelo retornado pelo GridSearchCV(). 
A GBDT também apresenta uma melhor curva ROC, estando mais localizada ao canto superior esquerdo do gráfico, sua AUC foi igual a 0.88.


<p align="center">
  <img src="figures/roccurve.png" width="500" height="350" />
</p>
