# Explicação


## Parte 1

### Atividade 1
- Calcular o macro_f1:

Macro_F1 é a média das notas f1 de todas as classes como a `numpy.average()` pois o retorno de `f1_por_classe.values()` é um dict com as notas f1 de cada classe.

- Calcular acurácia:

Acurácia é o número de elementos previsto corretamente dividido pelo número de elementos.

Para isso usamos a matriz de confusão onde a diagonal principal é a quantidade prevista corretamente: 

Conforme o exemplo: 
Onde linhas diz  a classe predita e a coluna a correta, então onde prevemos A e era realmente A é onde acertamos.


Nesse caso teríamos: (1+1+1)/(1+4+3+2+1+2+2+1+1)
3/17 = 0.1764 ou 17% de acurácia 

|   | A | B | C | 
|---|---|---|---|
| A | 1 | 4 | 3 |  
| B | 2 | 1 | 2 |  
| C | 2 | 1 | 1 |


### Atividade 2

Na atividade 2 foi implementado a classe eval.
Nesta classe separados o dataset `df_treino`  em duas partes `x_treino` e  `y_treino` que serão destinados a treinamento do modelo (`ml_method`), passado no construtor da classe. Para dividir esse dataset primeiro dropamos a `col_classe` que é a coluna identificadora da classe, e o resultado deste drop passamos para o `x_treino`, e em `y_treino` atribuímos apenas a coluna `col_classe`.

Treinamos nosso modelo usando o método `fit` presente nos classificadores e regressores da `sklearn`.

Após treinado o modelo, dividimos o dataset `df_data_to_predict` da mesma forma que o `df_treino` e passamos para o método `predict` do modelo treinado, que nos retorna um array com as predições.	

Por fim retornamos uma classe Resultado contendo os valores preditos e os valores reais.



### Atividade 3

Na atividade 3 foi implementado a classe fold, que é responsável por gerar os k-folds, métodos de treino e teste para avaliar os modelos.
Esse método consiste em realizar a validação cruzada, escolhendo um conjunto de treino e teste diferente a cada execução.


Inicialmente é criado um loop para executar `val_k` vezes, onde val_k é o número de folds que queremos gerar, ou o parâmetro k do k-fold.

Primeiramente embaralhamos o dataset `df_dados`. Em seguida buscamos o início e fim de cada fold, salvando nas variáveis:
- `ini_fold_to_predict` que será o número de instâncias por partição vezes o número do fold

- `fim_fold_to_predict` que será o início do fold mais o número de instâncias por partição

Seguindo separando o dataset `df_to_predict ` usando os índices `ini_fold_to_predict` e `fim_fold_to_predict` gerando assim o fold de validação desta execução do loop.

E geramos também o dataset de treino, dropando as linhas do `df_dados` que estão no `df_to_predict`.

Por fim criamos um objeto da classe `Fold` e adicionamos ele no nosso array de retorno.


## Parte 2
### Atividade 4

Nesta parte usamos a função do optuna trial, que sugere um valor para o parâmetro passado, neste caso:
- min_samples 
- max_features
- num_arvores

com estes valores sugeridos, criamos nosso `RandomForestClassifier`.

### Atividade 5

Na atividade 5, para cada fold gerado, criamos um novo study, da lib do [Optuna](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.Study.html#optuna.study.Study.optimize). Um estudo no optuna corresponde a uma tarefa de otimização, ou seja, um conjunto de tentativas.

Otimizamos com uma função objetivo, passando a função e o número de tentativas. 


Isso nos gera o número da melhor tentativa, e com isso usar a função `eval`, criada anteriormente, para gerar as predições do nosso modelo, salvando em um array de resultados.


### Atividade 6

Já na atividade 6 era preciso calcular o macro_f1 médio dos resultados obtidos na atividade 5.
Para isso buscamos o macro_f1 de cada resultado e salvamos em um array. Desta vez optei por usar a função `numpy.average()` para calcular a média.

