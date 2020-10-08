![Word2Vec-COVID19-Twitter](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/w2v.png?raw=true)
# Word2Vec-COVID19-Twitter

** **Trabalho em progresso** **

Modelo word2vec treinado com tweets brasileiros a respeito da pandemia de COVID-19.

![Nuvem de palavras mais frequentes](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/nuvem-tags.jpg?raw=true)

![Palavras similares](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/Figure_0.png?raw=true)

## Download do modelo

| Versão | Download | 
|------|:-------------------------:|
|`Word2Vec - BINÁRIO`  | [Download](https://drive.google.com/file/d/1VEL--MvQW49WDaf1_m3K7D9M60FD_N8D/view?usp=sharing) |
|`Word2Vec - MODEL`  | [Download](https://drive.google.com/file/d/1NzGu0_eTTdvaRMqxhfIsYZ9mgnvSuPsk/view?usp=sharing) |

## Como usar
-----
1. Faça o download do modelo e adicione na pasta <directory>.

2. Para usar o modelo binário:

```
w2v_model = KeyedVectors.load_word2vec_format("word2vec_tweets.bin", binary=True)
```
3. Para usar o modelo atual (model):

```
w2v_model = KeyedVectors.load("word2vec_tweets.model", binary=True)
```


## Como citar

** em breve **
