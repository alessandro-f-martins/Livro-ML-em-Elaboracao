# Indicadores para Aprendizado de Máquina e seu Uso na Seleção de Algoritmos para Análise de Dados



*Alessandro F. Martins*  
*Diretor de Consultoria*  
*AFM Estratégia de TI para Negócios*  

## Resumo



Cada demanda de negócio pede uma abordagem particular em relação ao tratamento dos dados para análise preditiva. Conhecendo-se a necessidade específica, é possível identificar a métrica mais adequada e usá-la como base para selecionar o algoritmo de aprendizado de máquina que a atenda da melhor forma.

Uma vez identificada a métrica apropriada, usam-se *metaalgoritmos* (ou métodos para otimização de *hiperparâmetros*) para buscar, no espaço de possíveis entradas (ou *espaço de fase*) de cada algoritmo, aqueles parâmetros que maximizam a capacidade do algoritmo de produzir os resultados desejados de previsão.

Neste capítulo, nossa tarefa é descrever os conceitos de cada métrica, em que contextos de negócio elas se fazem mais relevantes, e explorar os três principais métodos de otimização de hiperparâmetros utilizados por aplicações no mercado: *Grid Search*, *Randomized Search* e *Bayes Search*.

## Introdução

É importante ter-se em mente que *nenhuma métrica é perfeita*. Toda forma e conceito de medição acaba privilegiando alguma parte da informação que se quer trabalhar em detrimento de outros aspectos.


Analisemos três situações distintas em suas demandas e peculiaridades.

#### Análise de desistências (*Churn*)

No primeiro caso, temos uma empresa de serviços de telecomunicações (internet, telefonia fixa e móvel, TV por assinatura) que deseja reduzir sua perda de receita por *Churn* (desistência de clientes, com possível migração para a concorrência). Para isso, ela deseja obter as seguintes informações:

- Quais são as razões sob a responsabilidade da empresa que preponderantemente estão levando os clientes a deixar a empresa? São problemas técnicos, comerciais ou operacionais?

- Existem características específicas dos clientes com tendência a desistência? São fatores demográficos (idade, região, possui família, renda média), financeiros (pagamentos em atraso) ou outros (avaliação de conteúdo constantemente baixa, constante utilização do Serviço ao Cliente)?

- Quais são os clientes em minha base mais propensos à desistência no próximo ciclo de avaliação de *Churn*? Quais ações devem ser tomadas para mantê-los?

- Quais as melhores campanhas a se organizar para garantir a fidelização de meus clientes?

Consideremos os custos de retenção ativa, que correspondem a campanhas orientadas especificamente ao cliente em vias de desistência. Queremos minimizar esses custos focando apenas neste grupo, ao mesmo tempo que levamos em conta o (desejável) efeito colateral de que, fornecendo ao cliente satisfeito apenas a propaganda que lhe faça sentido, sua satisfação tende a ser maior. Logo, ao se lançar mão de métodos de análise preditiva utilizando aprendizado de máquina, queremos:

1. através de treinamento utilizando uma massa de dados com classes conhecidas de resultados, selecionar um algoritmo que *minimize a taxa de falsos negativos*;

2. ajustar corretamente seus hiperparâmetros para obter o resultado desejado;

3. usar o algoritmo assim otimizado para prever, em um massa de dados de clientes ainda não classificada, para quais clientes minha empresa deve realizar uma intervenção direcionada de marketing.

<xxx>

Logo, precisamos entender qual a importância de que o algoritmo e os hiperparâmetros selecionados privilegiem uma métrica como a *precisão*, pois ela indica a razão entre os positivos verdadeiros 

Tal

#### Diagnóstico médico assistido

No segundo,


#### Análise de fraudes em cartão de crédito

Finamente, análise de fraudes em cartão de crédito


## Métricas para Análise de Dados

### Nomenclatura inicial:

O processo de *validação* de um algoritmo e dos hiperparâmetros utilizados em seu ajuste fino é uma técnica bastante utilizada para estabelecer 

1. separa-se uma parte da massa de dados para a tarefa de validação. 25% é considerado uma boa prática para quantidades maiores de dados – mais de 5000 *data points* – enquanto massas menores devam ter uma fração maior a ser separada (por volta de 30% a 33%)

2. realiza-se o processo de treinamento com o restante dos dados. Internamente, a aplicação de aprendizado de máquina também divide os dados 


- **Positivos reais** ***(TP)***: dados previstos como sendo do caso positivo que, na validação, mostraram-se corretamente classificados

- **Negativos reais** ***(TN)***: dados previstos como sendo do caso negativo que, na validação, mostraram-se corretamente classificados

- **Falsos positivos** ***(FP)***: dados previstos como sendo do caso positivo que, na validação, mostraram-se incorretamente classificados, sendo realmente casos negativos

- **Falsos negativos** ***(FN)***: dados previstos como sendo do caso negativo que, na validação, mostraram-se incorretamente classificados, sendo realmente casos positivos 


### Descrição das métricas

- ***Acurácia***: probabilidade de sucesso do modelo de identificar os valores reais. Medida geral de êxito do modelo. Pode levar a conclusões errôneas em caso de desbalanceamento entre as classes observadas. Corresponde ao cálculo:

$$
ACC = \frac{TP + TN}{TP + TN + FP + FN}
$$

- ***Precisão***: probabilidade de sucesso do modelo de fazer uma classificação positiva correta. Relevante quando o custo de falsos positivos é alto.

$$
PRC = \frac{TP}{TP + FP}
$$

- ***Sensibilidade*** (ou *Recall*): medida da capacidade do modelo de identificar resultados positivos reais. Relevante quando o custo de falsos negativos é alto.

$$
REC = \frac{TP}{TP + FN}
$$

- ***F1***: média harmônica entre Precisão e Sensibilidade. Indica se há um baixo número de valores falsos (positivos e negativos), reduzindo a chance de alarmes falsos enquanto identifica bem os resultados reais. Mais relevante que a Acurácia, em especial no caso de uma distribuição de classes desbalanceada.

$$
F1 =  2×\frac{PRC × REC}{PRC + REC}   = \frac{2TP}{2TP + FP + FN}
$$

- ***ROC AuC*** (*Área sob a curva característica de operação do receptor*): medida da capacidade do modelo de separar exemplos positivos e negativos. Resume o *trade-off* entre a taxa de positivos verdadeiros e a taxa de falsos positivos.

- ***PRC AuC*** (*Área sob a curva de precisão/recall*): resume o *trade-off* entre a Precisão e a Sensibilidade do modelo. Especialmente relevante quando a razão de valores positivos e negativos no conjunto de dados é muito desbalanceada.

- ***MCC*** (*Coeficiente de correlação de Matthews*, ou *coeficiente fi ($\phi$)*): coeficiente de correlação entre as classificações observadas e previstas. Um valor de +100% representa uma previsão perfeita, 0 uma previsão não melhor que uma previsão aleatória e -100% um desacordo total entre previsão e observação. 

$$
MCC = \frac{TP × TN - FP × FN}{\sqrt{(TP + FP)(TP + FN)(TN + FP)(TN + FN)}}
$$

- ***CSI*** (*Índice crítico de sucesso*, também conhecido como ***TS***: *threat score*): fração de positivos verdadeiros que foram corretamente previstos. Equivalente a um cálculo de Acurácia em que não se levam em consideração os valores negativos reais.

$$
CSI = \frac{TP}{TP + FP + FN}
$$

- ***FNR*** (*Taxa de perdas*): equivale à probabilidade de se classificar uma condição positivo real como negativa.

$$
FNR = \frac{FN}{TP + FN}
$$

- ***FPR*** (*Taxa de alarmes falsos*): equivale à probabilidade de classificar uma condição negativa real como positiva.

$$
FPR = \frac{FP}{FP + TN}
$$

- ***FDR*** (*Taxa de descobertas falsas*): probabilidade do modelo de realizar uma classificação positiva incorreta. Complemento da Precisão.

$$
FDR = \frac{FP}{TP + FP}
$$

- ***FOR*** (*Taxa de omissões falsas*): probabilidade do modelo de realizar uma classificação negativa incorreta. Complemento do Valor Preditivo Negativo

$$
FOR = \frac{TN}{TN + FN}
$$


## Referências

\[1\]: Bernard Marr --  *What Is The Difference Between Artificial Intelligence And Machine Learning?*, em Forbes:  https://www.forbes.com/sites/forbesagencycouncil/2018/08/01/do-you-know-the-difference-between-data-analytics-and-ai-machine-learning/#3a8918cc5878

\[2\]: Vance Reavie -- *Do You Know The Difference Between Data Analytics And AI Machine Learning?*, em Forbes: https://www.forbes.com/sites/bernardmarr/2016/12/06/what-is-the-difference-between-artificial-intelligence-and-machine-learning/#7ed51bf72742

\[3\]: Stuart Russell, Peter Norvig -- *Artificial Intelligence – A Modern Approach (3^rd^ Edition)*, ISBN: 978-0-13-604259-4

