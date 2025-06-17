# Teste_Software_2025_Santana_Camille
Este repositório foi criado para testar e discutir um código disponibilizado como resposta aceita à pergunta **Python re.split() vs nltk word_tokenize and sent_tokenize** no Stack Overflow.
O script compara o desempenho e a precisão de três métodos de tokenização de palavras em Python:
- str.split(): método nativo, simples e extremamente rápido, mas com baixa precisão (não lida corretamente com pontuação);
- nltk.word_tokenize(): tokenizador baseado no modelo Penn Treebank, com alta precisão, porém mais lento;
- ToktokTokenizer (do NLTK): oferece um bom equilíbrio entre velocidade e precisão, segmentando pontuação corretamente com desempenho superior ao word_tokenize.
Além disso, o código inclui observações sobre a segmentação de sentenças utilizando nltk.sent_tokenize(), destacando que a avaliação adequada desse método requer um corpus anotado para comparação com resultados esperados.


