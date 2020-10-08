![Word2Vec-COVID19-Twitter](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/w2v.png?raw=true)
# Word2Vec-COVID19-Twitter

** **Trabalho em progresso** **

Modelo word2vec treinado com tweets brasileiros a respeito da pandemia de COVID-19. Corpus retirado do trabalho de [(Tiagode Melo, Carlos M.S.Figueiredo, 2020)] (https://www.sciencedirect.com/science/article/pii/S2352340920310738).

![Nuvem de palavras mais frequentes](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/nuvem-tags.jpg?raw=true)

![Palavras similares](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/Figure_0.png?raw=true)

## Download do modelo

| Versão | Download | 
|------|:-------------------------:|
|`Word2Vec - BINÁRIO`  | [Download](https://drive.google.com/file/d/1VEL--MvQW49WDaf1_m3K7D9M60FD_N8D/view?usp=sharing) |
|`Word2Vec - MODEL` (recomendado) | [Download](https://drive.google.com/file/d/1NzGu0_eTTdvaRMqxhfIsYZ9mgnvSuPsk/view?usp=sharing) |

## Como usar
-----
1. Faça o download do modelo e adicione na pasta corrente.

2. Para usar o modelo binário (use este modelo se for trabalhar com python 2.7):

```
w2v_model = KeyedVectors.load_word2vec_format("word2vec_tweets.bin", binary=True)
```
3. Para usar o modelo atual (model):

```
w2v_model = KeyedVectors.load("word2vec_tweets.model")
```


## Como citar

** em breve **
