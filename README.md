# Processamento de Linguagem Natural
Equipe:


- Camille Sousa Meneses de Santana
- Nayla Sahra Santos das Chagas
- Túlio Sousa de Gois
  
## Tutorial: Análise de Desempenho de Tokenizadores em Python

Neste tutorial exploramos a diferença de desempenho e funcionalidade entre métodos de tokenização em Python, utilizando a biblioteca `re` (expressões regulares) e a biblioteca NLTK (Natural Language Toolkit). A análise se baseia em uma pergunta do Stack Overflow escolhida previamente, e demonstra na prática como cada um das abordagens funciona.

### **Seleção da Pergunta**

Dentro do fórum StackOverflow, foram buscadas perguntas que possuíssem as tags: `nlp`, `python`, `regex` e `tokenize`.
Com base nos resultados, selecionamos somente os mais frequentes, o que resultou em 4 perguntas.

Das quatro perguntas disponíveis, selecionamos [Python re.split() vs nltk word_tokenize and sent_tokenize](https://stackoverflow.com/questions/35345761/python-re-split-vs-nltk-word-tokenize-and-sent-tokenize), cuja resposta indicada como correta possui 42 votos.

Dentro da pergunta, somos direcionados a um problema muito comum do processamento de linguagem natural: a tokenização.

### **Introdução ao Problema**

Em Processamento de Linguagem Natural (PLN), a **tokenização** é o processo de dividir um texto em unidades menores, chamadas tokens. Esses tokens podem ser palavras, sentenças ou mesmo caracteres. A escolha do método de tokenização pode impactar não apenas a precisão da análise subsequente, mas também o desempenho computacional.

A questão central da pergunta do usuário é: *O NLTK é mais rápido que as expressões regulares (regex) para tokenização de palavras e sentenças?*

Para responder a essa pergunta, compararemos `re.split()` e `str.split()` com as funções `word_tokenize` e `sent_tokenize` do NLTK.


### **Passo a Passo da Análise de Código**

O código foi desenvolvido para replicar e entender a solução proposta no Stack Overflow, além da solução original, tomamos a liberdade de acrescentar métodos que contribuem para uma resposta mais completa ao problema.

#### **1. Configuração do Ambiente e Comparação Inicial**

Primeiramente, é necessário instalar e importar as bibliotecas. O NLTK é a principal ferramenta adotada para esta análise.

```python
!pip install --quiet nltk
import nltk
nltk.download('punkt')
```

* **`!pip install --quiet nltk`**: Instala a biblioteca NLTK de forma silenciosa, ou seja, sem saídas na célula do notebook.
* **`import nltk`**: Importa a biblioteca para o ambiente.
* **`nltk.download('punkt')`**: Baixa o módulo `punkt`, que contém modelos pré-treinados para tokenização de sentenças.

Com o ambiente pronto, a primeira comparação envolve o método `split()` de strings e o `word_tokenize` do NLTK.

**Usando `str.split()`:**
Este método divide uma string com base em um delimitador (por padrão, o espaço em branco), mas não separa a pontuação das palavras, o que é uma limitação para análises linguísticas.

```python
sent = "This is a foo, bar sentence."
sent.split()
# Saída: ['This', 'is', 'a', 'foo,', 'bar', 'sentence.']
```
Como mostrado acima, temos tanto uma vírgula quanto um ponto fazendo parte dos tokens extraídos (`foo,` e `sentence.`).

**Usando `nltk.word_tokenize`:**
O `word_tokenize` utiliza um tokenizador mais sofisticado, o **Treebank**, que separa as palavras da pontuação, tratando-as como tokens distintos.

```python
from nltk import word_tokenize
word_tokenize(sent)
# Saída: ['This', 'is', 'a', 'foo', ',', 'bar', 'sentence', '.']
```

A diferença fundamental do segundo método, é que `word_tokenize` oferece um resultado linguisticamente mais útil, separando "foo," em "foo" e ",", expandindo o leque de possíveis análises.

#### **2. Medindo o Desempenho da Tokenização de Palavras**

Para uma comparação justa de desempenho, utilizaremos um conjunto de dados real com foco no tempo de execução dos processamentos. O código a seguir baixa um arquivo de texto e executa os dois métodos de tokenização 10 vezes cada, calculando o tempo médio.

```python
import time
import urllib.request

# URL do arquivo de dados de teste
url = 'https://raw.githubusercontent.com/Simdiva/DSL-Task/master/data/DSLCC-v2.0/test/test.txt'
response = urllib.request.urlopen(url)
data = response.read().decode('utf8')

# Teste para str.split()
for _ in range(10):
    start = time.time()
    for line in data.split('\n'):
        line.split()
    print ('str.split():\t', time.time() - start)

# Teste para word_tokenize()
for _ in range(10):
    start = time.time()
    for line in data.split('\n'):
        word_tokenize(line)
    print ('word_tokenize():\t', time.time() - start)
```

**Resultados do Desempenho:**
O `str.split()` é consistentemente mais rápido. Em média, `str.split()` levou cerca de **0.05 segundos**, enquanto `word_tokenize()` levou aproximadamente **4 segundos** para o mesmo conjunto de dados. Isso ocorre porque `word_tokenize` realiza operações mais complexas baseadas em regras linguísticas, enquanto `str.split()` apenas divide a string por espaços.

Isso significa que todos os métodos baseados em regras linguísticas serão absurdamente mais lentos? Bom, teríamos que testar métodos diferentes.

#### **3. Avaliando um Tokenizador Alternativo: TokTok**

Dentro do NLTK são disponibilizados outros tokenizadores. Na própria resposta do Stack Overflow somos convidados a testar o `ToktokTokenizer`, que é conhecido por ser mais rápido.

```python
from nltk.tokenize import ToktokTokenizer

toktok = ToktokTokenizer().tokenize

for _ in range(10):
    start = time.time()
    for line in data.split('\n'):
        toktok(line)
    print ('toktok:\t', time.time() - start)
```

O `ToktokTokenizer` apresenta um desempenho intermediário, com um tempo de execução médio de **1.5 segundos**. Ele é mais rápido que o `word_tokenize` padrão, mas ainda mais lento que o `str.split()`, mas ainda sim com resultado mais satisfatório para aplicações na linguística.

#### **4. A Complexidade da Tokenização de Sentenças (`sent_tokenize`)**

A comparação de velocidade para a tokenização de sentenças é mais complexa. Um método simples como `re.split()` pode ser rápido, mas sua precisão depende inteiramente da qualidade da expressão regular criada.

O `nltk.sent_tokenize()`, por outro lado, utiliza o algoritmo **Punkt**, que é um modelo de aprendizado de máquina não supervisionado para identificar sentenças. Ele é capaz de lidar com casos complexos, como abreviações (e.g., "Mr. Smith") e pontuação dentro de citações.

No código abaixo demonstramos a dificuldade de usar um método simples para um texto complexo.

```python
text = """In the third category he included those Brothers (the majority) who saw nothing in Freemasonry but the external forms and ceremonies..."""
answer = """In the third category he included those Brothers (the majority) who saw nothing in Freemasonry but the external forms and ceremonies, and prized the strict performance of these forms without troubling about their purport or significance.
..."""

# Usar split('\n') em um parágrafo contínuo não produzirá nenhuma sentença correta.
sum(1 for sent in text.split('\n') if sent in answer)
# Saída: 0
```

Quando aplicado ao mesmo texto, `sent_tokenize` consegue dividir corretamente o parágrafo em sentenças individuais:

```python
from nltk import sent_tokenize
sent_tokenize(text)
# Saída: ['In the third category he included those Brothers (the majority) who saw nothing in Freemasonry...', 'Such were Willarski and even the Grand Master of the principal lodge.', ...]
```

### **O que podemos concluir**

A resposta mais votada no Stack Overflow, e que seguimos neste tutorial, conclui que **a velocidade não deve ser o único critério para escolher um tokenizador**.

1.  **Para Tokenização de Palavras**: `str.split()` é mais rápido, mas inadequado para a maioria das tarefas de PLN, pois não lida com pontuação. `nltk.word_tokenize()` é a escolha recomendada para obter tokens linguisticamente corretos, apesar de ser mais lento. Para um meio-termo, o `ToktokTokenizer` é uma boa alternativa.

2.  **Para Tokenização de Sentenças**: Comparar a velocidade de `sent_tokenize()` com `re.split()` é complicado, pois a precisão é um fator crucial. Uma expressão regular simples pode falhar em textos complexos. O `nltk.sent_tokenize()` é mais robusto e confiável, pois é treinado para reconhecer os limites das sentenças em diversas situações.

#### **Por que outras abordagens não são ideais?**

Outras respostas poderiam sugerir o uso exclusivo de `re.split()` com expressões regulares complexas. Embora seja possível criar uma regex para tokenizar sentenças ou palavras, essa abordagem tem desvantagens:

* **Complexidade**: Criar e manter uma expressão regular que cubra todos os casos (abreviações, citações, etc.) é extremamente difícil.
* **Menor Generalização**: Uma regex que funciona para um tipo de texto pode falhar em outro. Os modelos do NLTK são pré-treinados em grandes volumes de dados e generalizam melhor.
* **Manutenção**: Se um novo caso de exceção aparecer, a regex precisa ser atualizada, enquanto o modelo do NLTK pode ser retreinado ou já cobrir tal caso.

Portanto, a solução correta enfatiza que a escolha depende do objetivo final: para análises linguísticas robustas, as ferramentas do NLTK são superiores, enquanto para divisões simples de strings, os métodos nativos do Python são mais eficientes.



