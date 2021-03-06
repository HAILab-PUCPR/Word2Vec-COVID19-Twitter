![Word2Vec-COVID19-Twitter](https://github.com/HAILab-PUCPR/Word2Vec-COVID19-Twitter/blob/master/w2v.png?raw=true)
# Word2Vec-COVID19-Twitter

** **Trabalho em progresso** **

Modelo Word2vec Continuos-Bag-Of-Words (CBOW) treinado com tweets brasileiros a respeito da pandemia de COVID-19, entre os meses de janeiro a maio de 2020. Corpus extraído a partir do trabalho de [(Tiagode Melo, Carlos M.S.Figueiredo, 2020)](https://www.sciencedirect.com/science/article/pii/S2352340920310738), utilizando biblioteca [twipy](https://pypi.org/project/twipy/).

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
from gensim.models import KeyedVectors

model = KeyedVectors.load_word2vec_format("word2vec_tweets.bin", binary=True)
```
Para usar o modelo atual (model):

```
from gensim.models import KeyedVectors

model = KeyedVectors.load("word2vec_tweets.model")
```

3. Para extrair 10 palavras mais frequentes:

```
from gensim.models import KeyedVectors

ponct = [':', ';', '!', '?', '.', ',', '[', ']', '@', '#', '(', ')', '/', '"', "'"]

w2v = dict()
for item in model.vocab:
#for item in model.wv.vocab: #gensim 1.0.0+

    it = item.lower()
    for let in it:
        if let in ponct:
            it = it.replace(let, '')
    if it == '':
        continue
    else:
        if it in w2v:
            w2v[it]+=model.vocab[item].count
            #w2v[it]+=model.wv.vocab[item].count #gensim 1.0.0+
        else:
            w2v[it]=model.vocab[item].count
            #w2v[it]=model.wv.vocab[item].count #gensim 1.0.0+
            
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
from wordcloud import WordCloud
stop_words = ["o", "a", "e", "da", "meu", "em", "você", "de", "ao", "os","nao"]

# wordcloud setup
wcBr = WordCloud(width = 800,
               height = 800,
               stopwords = stop_words,
               colormap = "Dark2",
               min_font_size = 10,
               max_words=40)
               
wcBr.generate_from_frequencies(frequencies = w2v)

import matplotlib.pyplot as plt

#plot
plt.figure(figsize = (8, 8), facecolor = None)
plt.imshow(wcBr, interpolation="bilinear")
plt.axis("off")
plt.savefig("tweets_wordcloud_bigram") #saves figure to dir
    
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

7. Para gerar gráfico t-SNE:

```

keys = ['coronavirus', 'covid', 'cloroquina', 'pandemia', 'quarentena', 'hidroxicloroquina', 'brasil', 'virus', 'casa',
        'lockdown']


embedding_clusters = []
word_clusters = []
for word in keys:
    embeddings = []
    words = []
    for similar_word, _ in model.most_similar(word, topn=10):
    #for similar_word, _ in model.most_similar(word, topn=5):
        words.append(similar_word)
        embeddings.append(model[similar_word])
    embedding_clusters.append(embeddings)
    word_clusters.append(words)


#codigo funciona
from sklearn.manifold import TSNE
import numpy as np

embedding_clusters = np.array(embedding_clusters)
n, m, k = embedding_clusters.shape
tsne_model_en_2d = TSNE(perplexity=15, n_components=2, init='pca', n_iter=3500, random_state=32)
embeddings_en_2d = np.array(tsne_model_en_2d.fit_transform(embedding_clusters.reshape(n * m, k))).reshape(n, m, 2)

import matplotlib.pyplot as plt
import matplotlib.cm as cm

def tsne_plot_similar_words(title, labels, embedding_clusters, word_clusters, a, filename=None):
    plt.figure(figsize=(16, 9))
    colors = cm.rainbow(np.linspace(0, 1, len(labels)))
    for label, embeddings, words, color in zip(labels, embedding_clusters, word_clusters, colors):
        x = embeddings[:, 0]
        y = embeddings[:, 1]
        plt.scatter(x, y, c=color, alpha=a, label=label)
        for i, word in enumerate(words):
            plt.annotate(word, alpha=0.5, xy=(x[i], y[i]), xytext=(5, 2),
                         textcoords='offset points', ha='right', va='bottom', size=16)
    #plt.legend(loc=4)
    plt.legend()
    plt.title(title)
    plt.grid(True)
    if filename:
        plt.savefig(filename, format='png', dpi=300, bbox_inches='tight')
    plt.show()


tsne_plot_similar_words('Similaridade entre palavras a partir de Tweets brasileiros sobre a COVID-19', keys, embeddings_en_2d, word_clusters, 0.7, 'similar_words_new.png')
```


## Como citar

```
@inproceedings{Giovanni-Paiva-etal-2020,
    title = "COVID 19: O que sentem os brasileiros de acordo com o Twitter?",
    author = "Paiva, Giovanni Pazini Meneghel  and
      Schneider, Elisa Terumi Rubel  and
      de Souza, Jo{\~a}o Vitor Andrioli  and
      Cezar, Josilaine Oliveira  and
      Oliveira, Lucas Ferro Antunes de  and
      Barra, Cl{\'a}udia Maria Cabral Moro and
      Paraiso, Emerson Cabrera  and
      Oliveira, Lucas Emanuel Silva e  and
      Gumiel, Yohan Bonescki",
booktitle = "CBIS 2020 - XVII Congresso Brasileiro de Informática em Saúde",
    year = "2020",
    address = "Online",
    publisher = "Journal of Health Informatics",
    url = "http://www.jhi-sbis.saude.ws/ojs-jhi/index.php/jhi-sbis/article/view/842",
    abstract = "Objetivo: A pandemia causada pelo novo coronavírus (SARS-CoV-2) caracteriza-se como o maior desafio do século 21. Neste contexto, procurou-se levantar um panorama geral de dados de usuários do Twitter, no Brasil, relacionados à COVID-19. Métodos: Utilizando de técnicas de Processamento de Linguagem Natural, foi aplicado um modelo Word2Vec CBOW em um conjunto pré-processado de dados públicos em português. Este foi então analisado através de wordclouds, tabelas e gráficos t-SNE. Resultados: O modelo captou comportamentos e tendências relacionados a COVID-19, como similaridades entre palavras, os unigramas e bigramas mais frequentes e hipóteses baseadas em dados estatísticos recolhidos. Conclusão: Este estudo apresenta uma análise inicial de mensagens do Twitter, em português, relacionadas à COVID-19. Os resultados foram promissores e evidenciaram o potencial da aplicação do aprendizado de máquina em assuntos importantes, como uma crise de saúde mundial.",
}
