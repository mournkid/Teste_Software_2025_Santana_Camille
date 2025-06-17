# Teste_Software_2025_Santana_Camille
Este repositório foi criado para testar e discutir um código disponibilizado como resposta aceita à pergunta **Python re.split() vs nltk word_tokenize and sent_tokenize** no Stack Overflow.
O script compara o desempenho e a precisão de três métodos de tokenização de palavras em Python:
- str.split(): extremamente rápido, mas não lida corretamente com pontuação;
- nltk.word_tokenize(): tokenizador baseado no modelo Penn Treebank, mais precisão em relação a pontuação, porém mais lento;
- ToktokTokenizer (do NLTK): atinge um bom equilíbrio entre velocidade e precisão.



