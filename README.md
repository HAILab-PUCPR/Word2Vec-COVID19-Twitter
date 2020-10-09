![Word2Vec-COVID19-Twitter](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/w2v.png?raw=true)
# Word2Vec-COVID19-Twitter

** **Trabalho em progresso** **

Modelo word2vec treinado com tweets brasileiros a respeito da pandemia de COVID-19. Corpus extraído a partir do trabalho de (Tiagode Melo, Carlos M.S.Figueiredo, 2020) (https://www.sciencedirect.com/science/article/pii/S2352340920310738).

Nuvem de Unigramas:

![Nuvem de palavras mais frequentes](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/nuvem-tags.jpg?raw=true)

![Palavras similares](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/Figure_0.png?raw=true)

## Download do modelo

| Versão | Arquitetura | Download | 
|------|------|:-------------------------:|
|`Word2Vec - BINÁRIO`  | CBOW | [Download](https://drive.google.com/file/d/1VEL--MvQW49WDaf1_m3K7D9M60FD_N8D/view?usp=sharing) |
|`Word2Vec - MODEL` (recomendado)| CBOW | [Download](https://drive.google.com/file/d/1NzGu0_eTTdvaRMqxhfIsYZ9mgnvSuPsk/view?usp=sharing) |

## Como usar
-----
1. Faça o download do modelo e adicione na pasta corrente.

2. Para usar o modelo binário (use este modelo se for trabalhar com python 2.7):

```
w2v_model = KeyedVectors.load_word2vec_format("word2vec_tweets.bin", binary=True)
```
Para usar o modelo atual (model):

```
w2v_model = KeyedVectors.load("word2vec_tweets.model")
```

3. Para extrair 10 palavras mais frequentes:

```
from gensim.models import KeyedVectors

ponct = [':', ';', '!', '?', '.', ',', '[', ']', '@', '#', '(', ')', '/', '"', "'"]

w2v = dict()
for item in model.vocab:
    it = item.lower()
    for let in it:
        if let in ponct:
            it = it.replace(let, '')
    if it == '':
        continue
    else:
        if it in w2v:
            w2v[it]+=model.vocab[item].count
        else:
            w2v[it]=model.vocab[item].count
            
w2vSorted=dict(sorted(w2v.items(), key=lambda x: x[1],reverse=True))

import pandas as pd

pd.set_option('max_colwidth',150)
data = pd.DataFrame.from_dict(w2vSorted.items())
data.columns = ['Word', 'Count']
data = data.sort_index()
data.head(10)



```

4. Para gerar nuvem de unigramas:


```
import matplotlib.pyplot as plt

#plot
plt.figure(figsize = (8, 8), facecolor = None)
plt.imshow(wc, interpolation="bilinear")
plt.axis("off")
plt.savefig("tweets_wordcloud_unigram") #saves figure to dir
    
plt.show()

```

5. Para extrair 10 bigramas mais frequentes::

```

# bigramas frequentes

bigr = {word : count for (word, count) in w2vSorted.items() for letter in word if (letter == '_')}

pd.set_option('max_colwidth',150)
bg = pd.DataFrame.from_dict(bigr.items())
bg.columns = ['Word', 'Count']
bg = bg.sort_index()
bg.head(10) #top 10

```

6. Para gerar nuvem de bigramas:

```

# wordcloud setup
wcBr = WordCloud(width = 800,
               height = 800,
               stopwords = stop_words,
               colormap = "Dark2",
               min_font_size = 10,
               max_words=40)
               
wcBr.generate_from_frequencies(frequencies = bigr)

import matplotlib.pyplot as plt

#plot
plt.figure(figsize = (8, 8), facecolor = None)
plt.imshow(wcBr, interpolation="bilinear")
plt.axis("off")
plt.savefig("tweets_wordcloud_bigram") #saves figure to dir
    
plt.show()

```


## Como citar

** em breve **
